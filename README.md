# xcode-vm

This project was developed in order to automate the builds of iOS and OS X apps
natively using Xcode but within a portable development VM. The result is a
packaged OS X VM that includes everything developers use to build iOS and OS X
apps natively on their desktop, except hosted from a remote, headless system.

You'll need to use your personal, licensed copy of OS X in order to build this
VM. Everything else will be automatically installed for you.

We'll use Vagrant but the Packer driven process should support output images
that run under many popular virtualization platforms (at least Virtualbox,
VMWare, etc). That magic is left to your system/network admin.

## Goal

The result of this entire process includes...

* Converting your licensed copy of OS X into a Vagrant base box.
* Running OS X as a VM for development and CD/CI processes.
* Scripted installation of Xcode and any other Apple dev dependency.
* Included [Fastlane](https://github.com/fastlane/fastlane) for build workflows.
* Included [Gym](https://github.com/fastlane/gym) for building and codesigning iOS apps.
* Installation of [Homebrew](http://brew.sh) and [Cocoapods](https://cocoapods.org) for package management.

## Requirements

* Homebrew
* w/ Brewfile support `brew tap homebrew/bundle`

## Setup

1. First, you'll need to make sure you've downloaded the latest version of OS X v10.11 (El Capitan).
  - Download OS X from the App Store
  - It'll end up in `/Applications/Install\ OS\ X\ El\ Capitan.app`
1. Next, we'll need to create the OS X VM base image for configuring under
Vagrant.
  - `boxcutter/osx` on GitHub does all the work here.
1. `git clone https://github.com/boxcutter/osx`
1. Next, install our local dependencies.
1. `brew bundle`
1. `sudo ./prepare_iso/prepare_iso.sh /Applications/Install\ OS\ X\ El\ Capitan.app dmg`
1. `packer build -only=virtualbox-iso -var-file=osx1011.json osx.json`
1. `vagrant box add ./box/virtualbox/osx1011-nocm-0.1.0.box --name osx1011`
1. ?
1. MASSIFY DA PROFITZ!

**WARNING:** This ain't done.

