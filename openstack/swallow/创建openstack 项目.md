[TOC]



# 如何从零创建一个openstack 项目

## 工具

 cookiecutter

## 创建新项目

```shell
pip install cookiecutter
#创建普通项目
cookiecutter -f https://git.openstack.org/openstack-dev/cookiecutter
#创建spec项目
cookiecutter -f https://git.openstack.org/openstack-dev/specs-cookiecutter
#创建oslo项目
cookiecutter -f https://git.openstack.org/openstack-dev/oslo-cookiecutter
#创建ui项目
cookiecutter -f https://git.openstack.org/openstack/ui-cookiecutter
```

## 参考链接

<https://github.com/openstack-dev/cookiecutter>

<https://docs.openstack.org/infra/manual/creators.html#preparing-a-new-git-repository-using-cookiecutter>

# 项目目录

## 新项目的目录树

```shell
├── babel.cfg
├── CONTRIBUTING.rst
├── demo
│   ├── __init__.py
│   └── tests
│       ├── base.py
│       ├── __init__.py
│       └── test_demo.py
├── doc
│   ├── requirements.txt
│   └── source
│       ├── admin
│       │   └── index.rst
│       ├── cli
│       │   └── index.rst
│       ├── configuration
│       │   └── index.rst
│       ├── conf.py
│       ├── contributor
│       │   ├── contributing.rst
│       │   └── index.rst
│       ├── index.rst
│       ├── install
│       │   ├── common_configure.rst
│       │   ├── common_prerequisites.rst
│       │   ├── get_started.rst
│       │   ├── index.rst
│       │   ├── install-obs.rst
│       │   ├── install-rdo.rst
│       │   ├── install.rst
│       │   ├── install-ubuntu.rst
│       │   ├── next-steps.rst
│       │   └── verify.rst
│       ├── library
│       │   └── index.rst
│       ├── readme.rst
│       ├── reference
│       │   └── index.rst
│       └── user
|		└── index.rst
├── HACKING.rst
├── LICENSE
├── README.rst
├── releasenotes
│   ├── notes
│   └── source
│       ├── conf.py
│       ├── index.rst
│       ├── _static
│       ├── _templates
│       └── unreleased.rst
├── requirements.txt
├── setup.cfg
├── setup.py
├── test-requirements.txt
└── tox.ini

```

