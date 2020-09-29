+++
title = "Installing Pathogen for Vim using Ansible"
date = "2020-09-29T15:19:01-04:00"
author = "Matt Higgins"
authorTwitter = "Mr_Higgi" #do not include @
cover = ""
tags = ["vim", "configuration", "linux"]
keywords = ["Ansible", "Vim", "Pathogen", "Automation"]
description = "Use Ansible to automate configuration of your dev environment. As an example, I show you how to automate configuration of Vim+Pathogen."
showFullContent = false
+++

If you want to maintain a base Vim configuration file while also having the ability to easily configure Vim for Pathogen package management in environments that will support it, you can do so with Ansible.

The Ansible script for this involves inserting a base .vimrc (or skipping this step if you have another method of inserting the .vimrc), and then installing Pathogen with infect() at the head of the .vimrc. This method accomplishes configuration from scratch, or addition of Pathogen to an existing configuration without destroying existing changes.

The [complete script](https://github.com/vextor22/ansible_scripts/blob/master/roles/dev/tasks/main.yml) is available on my [GitHub Ansible Repo](https://github.com/vextor22/ansible_scripts) for my basic dev environment. I recommend that if you don't already, create Ansible tasks for any configuration you commonly perform in a new environment. If you keep this up to date, you're only ever one ansible-playbook command away from a comfortable editor and toolset.
# Installing Pathogen

To install Pathogen via Ansible, we add the following tasks to the Ansible role:

```yaml
- name: Create pathogen dir
  shell: mkdir -p ~/.vim/autoload ~/.vim/bundle 
- name: Pull pathogen
  shell: curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```
These tasks create the expected paths for pathogen as well as pull the pathogen Vim module itself via curl.
# Adding Pathogen to .vimrc

To add Pathogen to the .vimrc, we want to first detect if Pathogen has already been added, and second add Pathogen to the head of the file if it isn't there already.

To do this, we can use the following tasks:

```yaml
- name: check for pathogen in vimrc
  shell: "cat ~/.vimrc | grep pathogen#infect\\(\\) | wc -l"
  register: pathogen_present

- debug: msg="{{pathogen_present.stdout}}"

- name: Add pathogen to vimrc if not present
  lineinfile:
      path: ~/.vimrc
      insertbefore: BOF
      line: "execute pathogen#infect()"
  when: pathogen_present.stdout == "0"
```

These tasks check for pathogen in the .vimrc file using grep to produce output if the configuration line exists. When the number of output lines, counted by wc -l, is 0, The next task will run, adding the Pathogen configuration to the .vimrc. To ensure the line is added to the beginning of the file we use insertbefore: BOF, where BOF means Beginning Of File.
# Using Pathogen with Ansible

Now that Pathogen is installed with Ansible, we want to install a Vim package to use it. The most visible package to install this way is [Lightline](https://github.com/itchyny/lightline.vim). The install for this package with Ansible and Pathogen is as simple as:

```yaml
- name: Vim - Install Lightline
  git:
      repo: git@github.com:itchyny/lightline.vim.git
      dest: ~/.vim/bundle/lightline.vim
      clone: yes
      update: yes

- name: check for laststatus in vimrc
  shell: "cat ~/.vimrc | grep laststatus=2 | wc -l"
  register: laststatus_present

- debug: msg="{{laststatus_present.stdout}}"

- name: Add laststatus to vimrc if not present to fix lightline
  lineinfile:
      path: ~/.vimrc
      line: "set laststatus=2"
  when: laststatus_present.stdout == "0"
```

After starting Vim you should see Lightline at the bottom of the screen. With this done, you are able to install whatever other Vim packages you'd like using Ansible. To see more examples, check out my Ansible script linked above.

