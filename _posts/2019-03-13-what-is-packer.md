---
layout: post
title: "ʲô��Packer"
subtitle: 'What is Packer?'
author: "cslqm"
header-style: text
tags:
  - Cloud
---


## Packer
Packer��һ���Զ����������־���ĽŲ����ߣ���golang��д��Ŀǰ��2019-03-15���Ѿ�֧��Docker��qemu��vmware�ĳ��þ������ͣ�����֧�ְ����ƣ�AWS��΢��Ŀ�洢�ӿڣ������ľ�������Զ����뵽�Ʒ���Ĵ洢�����ϡ�

## Packer���밲װ
�ٷ����ĵ����ڸ������������ǳ��򵥣���ĿĿ¼��ִ��"make dev"�������������»���ͦ�鷳�ġ���Ȼ������Packer��Ŀ�����޹أ�����golang���ú���ع��ߵ�ʹ�����⡣����ԭ������golang���֣�

��һֱ����û��д���Ƚϴ��golang���ӣ�ϰ���϶��ǽ�golang�ļ�������$GOPATH/src/my\_file�£����Զ���PackerҲ���������ġ�

	cd $GOPATH/src/my\_file
	git clone https://github.com/hashicorp/packer.git
	cd packer
	git checkout -b dev v1.3.5    #ѡ��ǰ������tag
	make dev                      #���ű���

֮�������ʾ�����İ������ڵ���Ϣ�������޷���ȡ(go get golang.org�ϵİ�)��

�����İ�̫���ˣ���ȫû����Ȥ��github�����ء�
��ã�У��ҷ���Packer��Ŀ�õ���govendor������һ���ǳ�ǿ���İ�����������������ȫ�����浽��ĿĿ¼�У�ʵ��һ��������Ŀ�������Ե������������е��������������õ���ǰ��Ŀ/vendorĿ¼�¡�(vendor:https://github.com/kardianos/govendor)
����Packer��ǰ���������е��������ˣ�Ϊ�λ���ȱ�ٰ��أ�����Ϳ����Ǳ�����Ҫ��Ҫ��װgovendor���������ʱ�Ұ�λ�ò��Ե����⡣

	go get -u github.com/kardianos/govendor #��װgovendor
֮���������$GOPATH/bin�·���һ����ִ���ļ�govendor��ֱ������ĿĿ¼��ִ��govendor���ܶ�vendorĿ¼�����еİ����й���

	govendor list #�鿴���еİ�
	+local    (l) packages in your project
	+external (e) referenced packages in GOPATH but not in current project
	+vendor   (v) packages in the vendor folder
	+std      (s) packages in the standard library

	+excluded (x) external packages explicitly excluded from vendoring
	+unused   (u) packages in the vendor folder, but unused
	+missing  (m) referenced packages but not found

	+program  (p) package is a main package

	+outside  +external +missing
	+all      +all packages

����Ϊ��������е����⣬���ĵ�ȥ�����˷��ֻ���̫���档
����ʱ�����Ҳ���ģ��github.com/hashicorp/packer���ⲻ�����Լ����������Ҫ�ҵ��Լ��������ʺţ���

Ȼ���Ҿͺ��ҳ��ԣ�����һ�����������Ӧ�ýй�� :P �������ǽ���ǰ��$GOPATH/src/my\_file/packerĿ¼�ƶ���$GOPATH/src/github.com/kardianos/govendor��

	mkdir $GOPATH/src/github.com/kardianos/
	mv $GOPATH/src/my\_file/packer $GOPATH/src/github.com/kardianos/
	cd $GOPATH/src/github.com/kardianos/
	make dev

���������ĵȴ���������$GOPATH/bin/�¿�����packer�ļ���




