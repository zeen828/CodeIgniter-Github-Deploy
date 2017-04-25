# CodeIgniter Github Deploy
---
本專案是基於![codeigniter_ico](https://raw.githubusercontent.com/zeen828/CodeIgniter-Github-Deploy/master/doc/ci_log.JPG)下開發的,  
可透過設定來達成GitHub自動部署程式碼到伺服器的目的.  

### 環境
---
* AWS Ubuntu 14.04  
* git 2.11.0  
* apache 2.4.7  
* PHP 5.6.30  
* CodeIgniter 3.1.4  

### 程式碼部署
---
1. 使用現有一個github的專案或是建立一個github專案clone下來  

        sudo git clone https://github.com/zeen828/CodeIgniter-Github-Deploy.git /var/www/deploy/

2. 下載本專案  
3. 編輯/application/config/sample/github_deploy.php後,將放置在是當的config位置  
4. 將/application/controllers/Deploy.php放置在controllers內  
5. 將網頁程式碼群組調整為www-data(apache預設用戶)  

        sudo usermod -a -G www-data ubuntu
        sudo chown -R www-data:www-data /var/www/deploy/

##### 會因系統不同apache預設角色群組不一樣,在aws的機器裡ubuntu是預設使用者所以這邊會將其納入

### 設定檔說明
        $config['git_path'] = '/usr/bin/git';
        $config['local_project_name'] = 'codeigniter-github-deploy';
        $config['github'] = array(
            'project name'=>array(
                'branch name'=>array(
                    'git_clean'=>false,
                    'secret_key'=>'Webhooks Secret',
                    'project_path'=>false,'project_path'=>'網頁伺服器絕對路徑',
                ),
            ),
        );
        $config['github_deploy_debug'] = true;

1. git_path : server上git的絕對位置  
2. local_project_name : 本地專案名稱(GitHub設定網址沒指定專案時會執行該專案)  
3. github : GitHub設定檔  
    * project name : GitHub專案名稱  
    * branch name : GitHub專案分支名稱  
    * git_clean : 是否清除不包含在該專案的檔案  
    * secret_key : GitHub在webhook設定的secret  
    * project_path : 專案絕對路徑  
4. github_deploy_debug : 除錯模式開關
##### config.php的要打開$config['log_threshold']為2或4才會寫log進CI的log目錄

### GitHub設定
---
1. 開啟一個專案(如果已有專案可略過)
![github project](https://raw.githubusercontent.com/zeen828/CodeIgniter-Github-Deploy/master/doc/github_project.JPG)

2. 點選setting選擇webhooks點選add webhooks
![github project settings](https://raw.githubusercontent.com/zeen828/CodeIgniter-Github-Deploy/master/doc/github_project_settings.JPG)

3. 填入網址和secret  
        http://localhost/index.php/deploy/receive?project=CodeIgniter-Github-Deploy&key=ci_deploy  
        http://網址/index.php/deploy/receive?project=GitHub專案名稱&key=GitHub在webhook設定的secret  
![github project settings webhooks](https://raw.githubusercontent.com/zeen828/CodeIgniter-Github-Deploy/master/doc/github_project_settings_webhooks.JPG)
