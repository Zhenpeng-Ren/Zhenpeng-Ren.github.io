---
title: ���ձʼǱ�װ˫ϵͳ��С��
categories: ����
date: 2017-10-29 20:22
comments: true
toc: true
rewards: true
tags:
	- Windows 10
	- ubuntu 14.04
---



## 0x01 ��������

���ڲ��뽫���յ���װ�����ϵͳ������ķ��ֻ��յ����Դ�һ��*ϵͳ�ָ�*�Ĺ��ܡ�

˫ϵͳ���õķ�ʽΪ��Windows 10 + Ubuntu 14.04



## 0x02 �����ձʼǱ�ִ��ϵͳ�ָ�����

���ȣ���Ҫ��ԭ��ϵͳ���������ñ��ݣ����ⶪʧ��

[ʹ�� HP Recovery Manager���лָ�](https://support.hp.com/cn-zh/document/c04777644)��

��� Windows �����򿪣���ִ�����²��裬ͨ�� Windows������������ϵͳ�ָ���
1. �رյ��ԡ�
2. �Ͽ����������豸�͵��£�����˱�ЯʽӲ�̡�U �̡���ӡ���ʹ���������ڲ���������ȡ�����ʣ���ж������������ӵ��ڲ�Ӳ����̨ʽ���Ļ�����Ҫ�Ͽ���ʾ�������̡����͵�Դ�ߵ����ӣ���
3. �������ԡ�
4. �ӡ����������棬���롰Recovery Manager����Ȼ������������ѡ��HP Recovery Manager����
5. �ڡ��������£����� Windows �ָ�������
6. �����ȷ������ϵͳ���������� Windows �ָ�������
7. ����������ų�����
8. �����Recovery Manager����
9. ѡ����û�б����ļ�������½��лָ�����Ȼ��������һ������
10. �Ķ�ϵͳ�ָ���Ϣ��Ҫ�������������� ����һ������
11. ���׼���������������������ť��

���ˣ����Խ��Զ�������������Ļ˵�������������ڴ��ڼ��жϲ������ڻָ��ڼ䣬���Ի�������Σ�֮��һ��ո�µ�windows 10ϵͳ��չ�ֳ����ˡ�



## 0x03 װ��Ubuntu 14.04ϵͳ

### 3.1 ׼��������������

1. ���� Ubuntu 14.04 [�ٷ�����](https://www.ubuntu.com/download/desktop)
2. [rufus](https://rufus.akeo.ie/?locale) \(��������U�̾���\)
3. ����4G��U��

### 3.2 ���̿ռ��������

���¡�WINDOWS��+��X����ϼ���������̹�����ѡ��һ���ռ���Ĵ��̣��Ҽ�ѡ��ѹ���������� Ubuntu ������50G��������

### 3.3 ���ÿ�������

==�� ���½��Ҽ����˵����ҵ���Դѡ��
==�� �����ѡ���Դ��ť�Ĺ��ܡ�
==�� ��������ĵ�ǰ�����õ����á�
==�� ȡ�������ÿ���������

ע��  �������������� Windows 10 Ϊ�˼����Լ����������̶�����һЩbios�ļ�顣

### 3.4 ���ð�ȫ������Secure Boot��

==�� ����������ѡ�񡰸��ºͰ�ȫ��
==�� ѡ�񡰻ָ���
==�� ѡ������������
==�� ѡ�����ѽ��
==�� ѡ�񡰸߼�ѡ�
==�� ѡ���������á�
==�� ѡ����Ӧ��ѡ��رհ�ȫģʽ����

Ҳ����ֱ�ӽ���BIOS���� Secure Boot �н�ֵ����Ϊ Disabled ���ɡ�

### 3.5 ����Ubuntu������U��

����U�̣���׼���õ� rufus ��ѡ��Ubuntu 14.04�ľ��񣬵����ʼ����д�룬�Ե�Ƭ�̵ȴ�д����ɡ�

### 3.6 U�̰�װUbuntu 14.04

���յ������ڿ���ʱ������ ESC �����ҵ� USB ���������Խ�����������

���е����ò���ϸ˵���������ؽ���һ�¡���װ���͡���һ����

1. ѡ������ѡ��
2. Ϊ���д��̷�����
	����һ���ῴ������֮ǰ�����δʹ�ô��̿ռ䣬���Ǽ���Ϊ�����д��̷�����
	/���洢ϵͳ�ļ�������10GB ~ 15GB��һ��16GB���ɣ�
	swap��������������Linuxϵͳ�������ڴ棬�����������ڴ��2����
	/home��homeĿ¼��������֡�ͼƬ�����ص��ļ��Ŀռ䣬��������������ʣ�µĿռ䣻
	/boot������ϵͳ�ں˺�ϵͳ����������ļ���ʵ��˫ϵͳ�Ĺؼ����ڣ�����200M

ע��ʵ���ϣ�һ��Ӳ���������4������������3�����������1����չ��������ѡ���������ʱ�����ܻ���֡���װϵͳʱ���з��������á�״����Ϊ�˽�����⣬����һ��ѡ���߼���������

3. ����Ҫ��һ����ѡ��/boot��Ӧ���̷���Ϊ����װ�������������豸������ر�֤һ�¡�

֮���������Ĳ�����ɰ�װ���ɡ�



## 0x04 ��ν���Ubuntuϵͳ

��װ�����������ֻ��ǽ����� win10 ϵͳ���޷����� Ubuntu ϵͳ����ʱ��һ���������Խ��� Ubuntu ϵͳ��������ʱ�� ESC+F9 ��������ѡ����棬���Կ����� win10 �� Ubuntu �������ѡ�� Ubuntu �Ϳ��������ˡ�

��Ȼ����ʹ��˫ϵͳ�������ǲ����㣬������ʱ��esc����һ��ͽ�����win10���о���һ��EFI�ļ�ϵͳ��EFI��һ�������ķ������ڵ�һ����̵Ŀ�ʼ256M�ռ��ϣ�ubuntu������EFI�ļ�ϵͳmount��/boot/efiĿ¼��

���ձʼǱ��� BIOS ������ָ���˴�EFI/Microsoft/Boot/bootmgfw.efi��������ʹEFI�»���������ϵͳ������Ҳ���������ϵͳ��������˿����޸Ĵ˴��ﵽ���ǵ�Ŀ�ġ�

�������£�

1. ���ȱ���win10���������Ŀ¼/usr/�£�
2. ��ͬһĿ¼�£�����WINDOWS�ļ��в�����Ϊwin10
3. ��/�е�grubx64.efi�滻��/�е�bootmgfw.efi�������滻֮��grub�ͽӹ���ϵͳ��������
4. ��һ���նˣ�ִ��update-grub2�������������������Ժ���/boot/grub/grub.cfg�ļ����ҵ���Ӧ��λ�ã���windows�ĳ�win10�Ϳ����ˡ�



## 0x05 ���

�ο����ӣ�
��1��[���տͻ�֧��](https://support.hp.com/cn-zh/document/c04777644)
��2��[����uefi��win10��Ubuntu 16.04 LTS˫ϵͳ����������grub2����](https://tinyslik.github.io/2017/02/10/win10+kylin%E5%8F%8C%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)
��3��[��Ӱ����2pro��װwin10+ubuntu16.10˫ϵͳ](http://blog.csdn.net/zyix_0712/article/details/69675748)