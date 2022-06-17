# devops-DZ3.1


## 1. Установите средство виртуализации Oracle VirtualBox.
    Установка версии 6.1 выполнена в Ubuntu 22.04
    sudo apt install virtualbox


## 2. Установите средство автоматизации Hashicorp Vagrant.

    Скачан бинарный архив vagrant_2.2.19_linux_amd64.zip.
    mkdir ~/vag
    cd ~/vag
    mv ~/Downloads/vagrant_2.2.19_linux_amd64.zip .
    unzip vagrant_2.2.19_linux_amd64.zip

## 3. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. 
    
    Буду использовать стандартные средства Ubunu 22.04

## 4. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните vagrant init. Замените содержимое Vagrantfile по умолчанию следующим:

         Vagrant.configure("2") do |config|
         	config.vm.box = "bento/ubuntu-20.04"
         end

Выполнение в этой директории vagrant up установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

vagrant suspend выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.
        
    Пришлось вручную скачать образ и добавить его в локально доступные 
    ./vagrant box add bentoo/ubuntu-20.04 bentoo_ubuntu_20.04

    box: Box file was not detected as metadata. Adding it directly...
    box: Adding box 'bentoo/ubuntu-20.04' (v0) for provider: 
    box: Unpacking necessary files from: file:///home/vk/vag/bentoo_ubuntu_20.04
    box: Successfully added box 'bentoo/ubuntu-20.04' (v0) for 'virtualbox'!

  ## 5.Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

    Создана VM с именем vagrant_default_1655497403769_26222
    Base memory  - 1 GB 
    2 CPU
    Virtual disk 64 GB (Actual Size 1,98 Gb) /home/vk/VirtualBox VMs/vagrant_default_1655497403769_26222/ubuntu-20.04-amd64-disk001.vmdk
    Network NAT

 ## 6.Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 4
    end

## 7.Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
    ~/vag/vagrant ssh
    Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)
    
    * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage
    
     System information as of Fri 17 Jun 2022 09:05:17 PM UTC
    
     System load:  0.01               Processes:             122
    Usage of /:   11.6% of 30.88GB   Users logged in:       0
    Memory usage: 18%                IPv4 address for eth0: 10.0.2.15
    Swap usage:   0%
    
    
    This system is built by the Bento project by Chef Software
    More information can be found at https://github.com/chef/bento

## 8.Ознакомиться с разделами man bash, почитать о настройках самого bash:

какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
        
    Переменная HISTSIZE, по умолчанию 500. 
    Manual page bash(1) line 862/4548
     
что делает директива ignoreboth в bash?

    ignorespace - строки, начинающиеся с пробела, не сохраняются в истории
    ignoredups - строки, соответствующие предыдущей записи истории, не сохраняются
    ignoreboth является сочетанием ignorespace и ignoredups
    Manual page bash(1) line 837/4548     
    
    

## 9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
    
       { list; }
              list is simply executed in the current shell environment.  list must be terminated with  a  newline  or
              semicolon.  This is known as a group command.  The return status is the exit status of list.  Note that
              unlike the metacharacters ( and ), { and } are reserved words and must occur where a reserved  word  is
              permitted  to be recognized.  Since they do not cause a word break, they must be separated from list by
              whitespace or another shell metacharacter.

    Manual page bash(1) line 257 

## 10.С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

    touch {00000..99999}.txt - успешно создается
    но 
    touch {000000..299999}.txt
    -bash: /usr/bin/touch: Argument list too long
    
    Это ограничение ядра на максимальную длину командной строки в байтах
    Можно прверить текущее значение
    
    vagrant@vagrant:~/ttt$ getconf ARG_MAX
    2097152

    

## 11.В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]] 

    [[ expression ]]
              Return a status of 0 or 1 depending on the evaluation of the conditional  expression  expression. 
    [[ выражение ]]
               Возвращает статус 0 или 1 в зависимости от оценки выражения условного выражения.         
    
    
    
## 12.Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

    bash is /tmp/new_path_directory/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash

    (прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.##

## 13. Чем отличается планирование команд с помощью batch и at?##

## 14. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.##
