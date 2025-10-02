# 推荐的参考资料

Ansible 英文视频教程: [`sysadmincasts.com/episodes/43-19-minutes-with-ansible-part-1-4`](https://sysadmincasts.com/episodes/43-19-minutes-with-ansible-part-1-4)

本书中所有 playbook 的例子都在 github 上： [`github.com/shijingjing1221/ansible-first-book-examples/`](https://github.com/shijingjing1221/ansible-first-book-examples/)

# YAML 语法基础

## 文件开始符

```
--- 
```

## 数组 List

```
- element1
- element2
- element3 
```

数组中的每个元素都是以 - 开始的。

## 字典(Hash or Directory)

```
key: value 
```

key 和 value 已冒号加空格分隔。

## 复杂的字典

字典的嵌套

```
# An employee record
martin:
    name: Martin D'vloper
    job: Developer
    skill: Elite 
```

字典和数组的嵌套

```
-  martin:
 name: Martin D'vloper
    job: Developer
    skills:
      - python
      - perl
      - pascal
-  tabitha:
    name: Tabitha Bitumen
    job: Developer
    skills:
      - lisp
      - fortran
      - erlang 
```

## 注意的地方

变量里有：要加引号

```
foo: "somebody said I should put a colon here: so I did" 
```

变量的引用要加引号

```
foo: "{{ variable }}" 
```

## 参考资料

[`en.wikipedia.org/wiki/YAML`](https://en.wikipedia.org/wiki/YAML)

[`www.yamllint.com/`](http://www.yamllint.com/)

# 待续

## Ansible Tower 简介

[`www.ansible.com/tower`](https://www.ansible.com/tower)

## 编写自己的 Ansible module