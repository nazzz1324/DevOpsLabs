
# Отчет по лабраторной работе на тему "Автоматизация сборки и деплоя Python Docker-приложений через Jenkins с интеграцией GitHub"
***

## Введение

Цель данной лабораторной работы — освоить практические навыки работы с Ansible, инструментом для автоматизации конфигурации и управления инфраструктурой. В рамках работы предстояло настроить управляющую машину с Ansible, развернуть управляемый хост в Docker-контейнере, настроить SSH-подключение без пароля, создать инвентарный файл и выполнить 

***
## Ход работы
### Задание № 1
Установили python и ansible
<img width="945" height="888" alt="image" src="https://github.com/user-attachments/assets/317e32fb-b546-4e13-97e9-370ec7f798e1" />
<img width="1380" height="232" alt="image" src="https://github.com/user-attachments/assets/cd3af2f8-b8f6-40bc-a8ba-9ba8ef382442" />

Сгенерировали SSH ключевой пары на управляющей машине
<img width="1043" height="413" alt="image" src="https://github.com/user-attachments/assets/2a1fe303-15e0-4508-aa0a-7db053442c52" />

Созданили Dockerfile для управляемого хоста, docker-compose.yml и собрали и запустили контейнер
<img width="943" height="299" alt="image" src="https://github.com/user-attachments/assets/a65e871a-4dbf-4dbd-821b-d7a9fbb5dbea" />

Проверили SSH подключение к контейнеру
<img width="950" height="460" alt="image" src="https://github.com/user-attachments/assets/03a99af9-0382-48fe-8291-511b48efe700" />

Создали и проверили инвентарный файл Ansible (inventory)
<img width="937" height="615" alt="image" src="https://github.com/user-attachments/assets/453cc11e-2f44-412b-aa49-6e17b7080b10" />

Проверили подключение Ansible к управляемому хосту
<img width="955" height="182" alt="image" src="https://github.com/user-attachments/assets/2f0bdf3e-2f12-4402-8bed-259b547254e6" />
<img width="934" height="736" alt="image" src="https://github.com/user-attachments/assets/545bc58f-683e-43a0-9489-055d47cef7b0" />
<img width="929" height="110" alt="image" src="https://github.com/user-attachments/assets/9dc97f31-a501-463d-99c2-4da10161e6cf" />

***
### Задание №2

Получили информацию о ядрах CPU управляемого хоста, список всех пользователей и проверили свободное место на диске:
<img width="1244" height="930" alt="image" src="https://github.com/user-attachments/assets/88b9cb96-07a0-4ed8-ae64-1472026eaf3a" />
Изменили временную зону хоста на UTC:
<img width="1095" height="155" alt="image" src="https://github.com/user-attachments/assets/c80038c6-d379-4212-8114-2c5124b4790a" />

***
### Задание №3

Создайли новый playbook `task3_files.yml`:

 ```yaml
   ---
   - name: Work with files
     hosts: managed_hosts
     tasks:
       - name: Create multiple directories
         file:
           path: /tmp/{{ item }}
           state: directory
           mode: '0755'
         loop:
           - test_dir1
           - test_dir2
           - test_dir3
   
       - name: Create files in directories
         copy:
           content: "This is {{ item }} file\n"
           dest: /tmp/{{ item }}/content.txt
         loop:
           - test_dir1
           - test_dir2
           - test_dir3
   
       - name: Display files
         command: cat /tmp/{{ item }}/content.txt
         loop:
           - test_dir1
           - test_dir2
           - test_dir3
         register: file_content
   
       - name: Show file contents
         debug:
           msg: "{{ item.stdout }}"
         loop: "{{ file_content.results }}"
   ```

и запустили его:
<img width="1280" height="606" alt="image" src="https://github.com/user-attachments/assets/4c4e6919-b10e-4a61-868c-90249152dc82" />

