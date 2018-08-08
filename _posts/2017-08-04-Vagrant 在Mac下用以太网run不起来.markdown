---
layout: post
title:  "Vagrant 在Mac下用以太网run不起来"
date:   2017-08-04 15:01:24 +0800
categories: Vagrant
tags: Vagrant
---

报错信息如下:

```bash
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set network interfaces...
==> default: Available bridged network interfaces:
1) en0: 以太网
2) en2: Thunderbolt 1
3) en3: Thunderbolt 13
4) bridge0
==> default: When choosing an interface, it is usually the one that is
==> default: being used to connect to the internet.
    default: Which interface should the network bridge to? 1
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: bridged
/opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:29:in `encode': "\xE4" from ASCII-8BIT to UTF-8 (Encoding::UndefinedConversionError)
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:29:in `block in initialize'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:29:in `map'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:29:in `initialize'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:22:in `new'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:22:in `execute'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/driver/base.rb:430:in `block in raw'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/busy.rb:19:in `busy'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/driver/base.rb:429:in `raw'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/driver/base.rb:367:in `block in execute'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/retryable.rb:17:in `retryable'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/driver/base.rb:362:in `execute'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/driver/version_5_1.rb:233:in `enable_adapters'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/action/network.rb:118:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/action/clear_network_interfaces.rb:26:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/providers/virtualbox/action/prepare_nfs_settings.rb:18:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /Users/richard/.vagrant.d/gems/2.2.5/gems/vagrant-proxyconf-1.5.2/lib/vagrant-proxyconf/action/only_once.rb:21:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/builtin/synced_folders.rb:87:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/builtin/synced_folder_cleanup.rb:28:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/plugins/synced_folders/nfs/action_cleanup.rb:25:in `call'
	from /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/action/warden.rb:34:in `call'
	....
```

发现错误文件地址:
```bash
/opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb:28:in `encode': "\xE4" from ASCII-8BIT to UTF-8 (Encoding::UndefinedConversionError)
```
发现是编码问题

进入错误文件:

```bash
sudo vim /opt/vagrant/embedded/gems/gems/vagrant-1.9.1/lib/vagrant/util/subprocess.rb
```

光标移到28行修改为:

原来的:
```ruby
@command = @command.map { |s| s.encode(Encoding.default_external) }
```

修改后:
```ruby
@command = @command.map { |s| s.force_encoding('UTF-8') }
```

保存，然后重新把`vagrant`运行