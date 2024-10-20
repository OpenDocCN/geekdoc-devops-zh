# 我学到的关于可转换标签的 3 件事(艰难的方式)

> 原文：<https://www.fairwinds.com/blog/3-things-ive-learned-about-ansible-the-hard-way>

 Ansible 标记的简单要求使它非常容易上手。总的来说，它工作得非常好，但是一旦你深入一点，有些事情可能会导致不适。以下是我艰难地了解到的关于(或重新了解)的三件事。

## 标签并不像你预期的那么远

可变标签 是一种限制工作量的有效方法。一般来说，行动手册将贯穿始终，因为您使它们幂等，但有时只针对非常具体的部分也不错。毕竟，当您只是想改变 Nginx 中的设置时，为什么要运行所有与数据库相关的任务呢？

很明显，ansible 标签对于简化工作非常有用，但是它们有一个非常明显的限制。一旦播放器匹配一个标签，它也将运行作为该播放器一部分的每个任务，而不管其他标签。

让我们看一个例子。以下是一个非常简单的顶级玩法:

```
- include: sub.yml
  tags: top 
```

那个文件包括下面的  `sub.yml`:

```
- hosts: localhost

  tasks:
    - debug: msg="sub and top"
      tags: sub,top

    - debug: msg="sub"
      tags: sub

    - debug: msg="blank" 
```

运行  `ansible-playbook` 而没有任何  `--tags` 产生所有的调试消息(这应该很明显，因为 Ansible 的默认是  `--tags all`)。运行`ansible-playbook``--tags top`也会产生所有的输出:

```
$ ansible-playbook --tags top ./top.yml

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "sub and top"
}

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "sub"
}

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "blank"
}

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0 
```

该信息在文档中有所提及，尽管它并没有详细说明这一点:

> 所有这些都将指定的标签应用到剧本、包含的文件或角色中的每个任务，以便当剧本被相应的标签调用时，这些任务可以选择性地运行

因此，如果您希望在包含的游戏中使用 ansible 标记来继续隔离事物，您将会失望。

通过添加  `--skip-tags`，可以进一步限制执行时发生的情况:

```
$ ansible-playbook --tags top --skip-tags sub ./top.yml

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "blank"
}

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0 
```

在这种情况下，模糊的第一个任务在  `sub.yml` 中匹配而不匹配。效果是它不运行。

最后，如果跳过顶层，什么都不会发生:

```
$ ansible-playbook --tags sub --skip-tags top ./top.yml                                 

PLAY [localhost] ***************************************************************

PLAY RECAP ********************************************************************* 
```

`--tags`还有很多动力，但是我知道我花了太多时间去弄清楚为什么有些东西会运行或者不运行。根据经验，我尝试将 ansible 标记与 role 和 include 语句一起使用。如果事情需要敏感对待，我一般会设置一个布尔变量，默认为  `false`或者  `no` 而不是让事情偶然发生。

## 除非你踩在上面，否则杂凑就是杂凑

由于  `replace`(默认)或  `merge`的散列合并行为，在 Ansible 中使用散列可能会很棘手。

这意味着什么呢？让我们举个例子:

```
mj:
    name: M Jay
    job: hash merger
    skill: intrepid 
```

假设我们想要通过向  `mj`添加另一个位来增加散列:

```
mj:
  eye_color: blue 
```

使用  `replace`的默认行为，您最终将只有  `mj`的眼睛颜色。在  `ansible.cfg` 中设置  `hash_behaviour = merge` 会得到一个散列，它拥有  `mj`的所有属性。

现在，覆盖配置文件中的设置的问题是，它可能会反咬你一口。如果该文件被篡改或在另一个系统上运行，等等。，你可能会得到一些非常奇怪的效果。Ansible 2 中的变通方法是使用  `combine` 过滤器来显式合并。

这个题目上的  [文献](http://docs.ansible.com/ansible/playbooks_filters.html#combining-hashes-dictionaries) 都不错。对于上面的例子，它看起来像这样:

```
- hosts: localhost

  vars:
    var1:
      mj:
        name: M Jay
        job: hash merger
        skill: intrepid
    var2:
      mj:
        eye_color: blue

  tasks:
    - debug: var=var1
    - debug: var=var2
    - set_fact:
      combined_var: "{ { var1 \ combine(var2, recursive=True) } }"
    - debug: var=combined_var 
```

您将得到以下结果:

```
$ ansible-playbook hash.yml                                                              

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "var1": {
        "mj": {
            "job": "hash merger",
            "name": "M Jay",
            "skill": "intrepid"
        }
    }
}

TASK [debug] *******************************************************************
ok: [localhost] => {
    "var2": {
        "mj": {
            "eye_color": "blue"
        }
    }
}

TASK [set_fact] ****************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "combined_var": {
        "mj": {
            "eye_color": "blue",
            "job": "hash merger",
            "name": "M Jay",
            "skill": "intrepid"
        }
    }
}

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=0    unreachable=0    failed=0 
```

还是那句话，我的建议:不要改变默认，要学会热爱  `combine` 滤镜。

## set_fact 很粘

Ansible 中设置变量的方式有多种:  `include_vars`，  `vars:`，将它们作为 play 的一部分传递，当然还有  `set_fact`。需要意识到的是，一旦你使用  `set_fact` 来设置一个变量，你就不能改变它，除非你再次使用  `set_fact` 。我认为从技术上来说事实优先更正确，但效果是一样的。举以下例子:

```
# note that the included play only contains a debug
# statement like the others listed in the example below.

- hosts: localhost

  vars:
    var1: foo

  tasks:
    # produces foo (defined in vars above)
    - debug: msg="original value is "

    # also produces foo
    - include: var_sub.yml

    # produces bar (since it's being passed in)
    - include: var_sub.yml var1=bar

    # produces foo (we're back to the original scope)
    - debug: msg="value after passing to include is "

    # now it get's interesting
    - set_fact:
        var1: baz

    # produces baz
    - debug: msg="value after set_fact is "

    # also produces baz
    - include: var_sub.yml

    # baz again!!! since set_fact carries the precedence
    - include: var_sub.yml var1=bar

    # using set_fact we can change the value
    - set_fact:
        var1: bat

    # the rest all produce bat
    - debug: msg="value after set_fact is "
    - include: var_sub.yml
    - include: var_sub.yml var1=bar 
```

寓意是，当你看到一些奇怪的行为时，运行  `-vvv` 或添加  `debug` 任务是一个好主意。

## 新事物:将角色作为任务运行！

我很兴奋地得知，有了 Ansible 版，将角色作为任务运行成为可能。这听起来可能不多，但它非常强大，可以节省我以前编写和/或复制的许多奇怪的代码。

魔咒是  [包含 _ 角色](https://docs.ansible.com/ansible/include_role_module.html)。

这个新特性非常强大，允许您编写更小的角色，以便于包含。例如，您可以轻松创建一个角色来管理您的数据库表或 Elasticsearch 索引。我遇到过一些用例。有了这个新的重头戏，现在可以做这样的事情:

```
- include_role: create-db
  with_items:
    - 'test1'
    - 'tester'
    - 'monkey'
    - 'production' 
```

它太强大了，我简直不敢相信它很久以前就不存在了。

现在，要是我能在  `block` s 上循环就好了…

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)