# seoimo.com的自动备份和还原WordPress脚本

>   首先，安装备份脚本：
>
>   \# cd /home/wwwroot/ && wget https://www.seoimo.com/downloads/shells/wp-backup-seoimo.sh
>
>    脚本内容如下：
>
>   ```shell
>   #!/bin/bash
>   
>   # 相关变量(修改以下关于远程FTP、备份名称和备份保留次数的信息)
>   FTP_IP=111.111.111.111
>   FTP_USER="ftpusername"
>   FTP_PASSWD="ftpuserpasswd"
>   BACKUP_NAME="seoimo-wp-backup"
>   BACKUP_NUMBER=5
>   MYSQL_ROOT_PASSWD="mysqlrootpasswd"
>   FTP_BACKUP="/home/backup"
>   DIR_BACKUP="/home/backup"
>   
>   # 以下参数切勿修改，除非你真的需要
>   NEW_BACKUP="$BACKUP_NAME-$(date "+%Y-%m-%d")"
>   OLD_BACKUP=$(ls $DIR_BACKUP | grep -E "$BACKUP_NAME" | grep -E ".tar.gz" | sort -nr | tail -n +$BACKUP_NUMBER)
>   FONT_GREEN="\033[32m"
>   FONT_RED="\033[31m"
>   FONT_SUFFIX="\033[0m"
>   INFO="${FONT_GREEN}[INFO]${FONT_SUFFIX}"
>   ERROR="${FONT_RED}[ERROR]${FONT_SUFFIX}"
>   DIR_HTTPD="/usr/local/apache/conf"
>   DIR_DATABASE="/usr/local/mysql/var"
>   DIR_WWWROOT="/home/wwwroot"
>   DIR_ACMESH="/usr/local/acme.sh"
>   DIR_CRONTAB="/var/spool/cron"
>   echo -e "${FONT_GREEN}
>   # ====================================
>   # Name: WordPress-Backup by SEOIMO
>   # Link: https://www.seoimo.com/wordpress-backup/
>   # Ver.: 1.0.0
>   # ====================================
>   ${FONT_SUFFIX}"
>   
>   # 创建备份目录
>   if [ ! -d "$DIR_BACKUP" ]; then
>     mkdir -p "$DIR_BACKUP"
>   fi
>   
>   # 创建临时备份目录
>   echo -e "[Starting time: `date +'%Y-%m-%d %H:%M:%S'`]"
>   TIME_START=$(date +%s)
>   cd $DIR_BACKUP && mkdir -p $NEW_BACKUP
>   
>   # 列出已绑定域名对应的数据库信息
>   WP_DOMAIN=$(ls -F $DIR_WWWROOT | grep "/$" | awk -F "/" '{print $1}')
>   for wp in $WP_DOMAIN; do
>     if [ -f "$DIR_WWWROOT/$wp/wp-config.php" ]; then
>       DB_USER=$(grep "DB_USER" $DIR_WWWROOT/$wp/wp-config.php | awk -F "'" '{print $4}')
>       DB_PASSWORD=$(grep "DB_PASSWORD" $DIR_WWWROOT/$wp/wp-config.php | awk -F "'" '{print $4}')
>       printf "%-20s %-20s %-25s\n" $DB_USER $DB_PASSWORD $wp >> db-info-all-list.tmp
>     fi
>   done
>   
>   # 逐个备份数据库信息
>   DB_NAME=$(awk '{print $1}' db-info-all-list.tmp)
>   for db in $DB_NAME; do
>     mysqldump -uroot -p"$MYSQL_ROOT_PASSWD" $db > $db-$(date "+%Y-%m-%d").sql
>   done
>   
>   # 复制备份和打包压缩
>   mv *.sql $NEW_BACKUP
>   \cp -rdpv $DIR_HTTPD $DIR_DATABASE $DIR_WWWROOT $DIR_ACMESH $DIR_CRONTAB $NEW_BACKUP
>   tar -zcvf $NEW_BACKUP.tar.gz $NEW_BACKUP
>   rm -rf $NEW_BACKUP && rm -rf $OLD_BACKUP && rm -rf db-info-*-list.tmp
>   
>   # 通过FTP远程备份
>   CUR_BACKUP_NUM=$(ls $DIR_BACKUP | grep -E "$BACKUP_NAME" | grep -E ".tar.gz" | sort -nr | wc -l)
>   if [ "$CUR_BACKUP_NUM" -ge "$BACKUP_NUMBER" ]; then
>     for it in $OLD_BACKUP; do
>       ftp -i -n -v << END
>         open $FTP_IP
>         user $FTP_USER $FTP_PASSWD
>         binary
>         mdelete $it
>         lcd $DIR_BACKUP
>         hash
>         prompt off
>         mput $NEW_BACKUP.tar.gz
>         close
>       bye
>   END
>     done
>   else
>     ftp -i -n -v << END
>       open $FTP_IP
>       user $FTP_USER $FTP_PASSWD
>       binary
>       lcd $DIR_BACKUP
>       hash
>       prompt off
>       mput $NEW_BACKUP.tar.gz
>       close
>     bye
>   END
>   fi
>   
>   # 计算耗时
>   echo -e "[End time: `date +'%Y-%m-%d %H:%M:%S'`]"
>   TIME_END=$(date +%s)
>   echo -e "${FONT_GREEN}[Successfully done! Command takes $((TIME_END-TIME_START)) seconds.]${FONT_SUFFIX}"
>   echo -e "$INFO If you get a problem, please leave a message at https://www.seoimo.com/wordpress-backup/"
>   
>   # 退出
>   exit 0
>   
>   ```
>
>   然后，修改相关信息（具体可参看文中说明）：
>
>   \# vi /home/wwwroot/wp-backup-seoimo.sh
>
>    
>
>   最后，设置定时备份（例如每两天/每隔一天的凌晨00:20）：
>
>   \# echo -e "20 0 */2 * * bash /home/wwwroot/wp-backup-seoimo.sh" >> /var/spool/cron/root
>
>    
>
>   当然，你也可以自定义备份频率，比如每天或者每三天等。
>
>   只需把上面命令中的 ***/2** 改成 ***/1** 或者 ***/3** 即可。
>
>   但是，建议不要间隔太长，每天或者每两天备份一次比较好。
>
>    
>
>   好了，你可以查看已经成功添加了定时备份任务：
>
>   \# crontab -l
>
>    
>
>   现在，你可以尝试手动备份一次：
>
>   \# bash /home/wwwroot/wp-backup-seoimo.sh
>
>    
>
>   查看备份目录，已经新建了一份备份：
>
>   \# ls -la /home/backup/
>
>    
>
>   \-------------------------------
>
>    
>
>   如果需要把当前VPS上 **所有WordPress** 恢复到最新备份状态，
>
>   或者搬家/迁移后在新VPS上恢复，可以使用如下脚本：
>
>   \# cd /root/ && wget https://www.seoimo.com/downloads/shells/wp-restore-seoimo.sh
>
>    脚本内容如下：
>
>   ```shell
>   #!/bin/bash
>   
>   # 相关变量（默认即可）
>   DIR_BACKUP="/home/backup"
>   FONT_GREEN="\033[32m"
>   FONT_RED="\033[31m"
>   FONT_SUFFIX="\033[0m"
>   INFO="${FONT_GREEN}[INFO]${FONT_SUFFIX}"
>   ERROR="${FONT_RED}[ERROR]${FONT_SUFFIX}"
>   DIR_HTTPD="/usr/local/apache/conf"
>   DIR_DATABASE="/usr/local/mysql/var"
>   DIR_WWWROOT="/home/wwwroot"
>   DIR_ACMESH="/usr/local/acme.sh"
>   DIR_CRONTAB="/var/spool/cron"
>   echo -e "${FONT_GREEN}
>   # ====================================
>   # Name: WP-Restore by SEOIMO
>   # Link: https://www.seoimo.com/wordpress-backup/
>   # Ver.: 1.0.0
>   # ====================================
>   ${FONT_SUFFIX}"
>   
>   # 创建备份目录
>   if [ ! -d "$DIR_BACKUP" ]; then
>     mkdir -p "$DIR_BACKUP"
>   fi
>   
>   # 选择备份文件的位置（本地VPS还是远程FTP）
>   echo -e "Please choose where to restore a backup: \n${FONT_GREEN}1 - Remote FTP server (default) \n2 - Local VPS server ${FONT_SUFFIX}"
>   read -p "Please enter your choice/number (1 or 2): " SELECT_NUMBER
>   if [ "$SELECT_NUMBER" == "2" ]; then
>     # 选择要恢复的备份文件（从本地恢复）
>     echo -e "--------------------------------------"
>     echo -e "All backup files in local "$DIR_BACKUP" directory below: \n${FONT_GREEN}$(ls $DIR_BACKUP | grep -E ".tar.gz")${FONT_SUFFIX}"
>     echo -e "--------------------------------------"
>     read -p "Please select one backup you want to restore (e.g. xxx.tar.gz): " SELECT_BACKUP
>     # 起始时间
>     echo -e "${FONT_GREEN}Start restoring now..${FONT_SUFFIX}"
>     echo -e "[Starting time: `date +'%Y-%m-%d %H:%M:%S'`]"
>     TIME_START=$(date +%s)
>   elif [ "$SELECT_NUMBER" == "1" ] || [ -z "$SELECT_NUMBER" ]; then
>     # FTP登入信息（从FTP端恢复）
>     echo -e "${FONT_GREEN}Some important information below, you must enter more carefully!${FONT_SUFFIX}"
>     read -p "Please enter the IP of FTP server (e.g. 158.158.158.158): " FTP_IP
>     read -p "Please enter the User of FTP server (e.g. imbackup): " FTP_USER
>     read -p "Please enter the Password of FTP user (e.g. 1234567890): " FTP_PASSWD
>     # 建立临时文件
>     cd $DIR_BACKUP && echo -e "" > nlist-ftp.tmp
>     # 登入FTP获取备份目录
>   ftp -i -n -v << END
>   open $FTP_IP
>   user $FTP_USER $FTP_PASSWD
>   binary
>   lcd $DIR_BACKUP
>   hash
>   prompt off
>   nlist . nlist-ftp.tmp
>   close
>   bye
>   END
>     # 选择要恢复的备份文件
>     echo -e "--------------------------------------"
>     echo -e "All backup files in remote FTP server below: \n${FONT_GREEN}$(grep -E ".tar.gz" nlist-ftp.tmp)${FONT_SUFFIX}"
>     echo -e "--------------------------------------"
>     read -p "Please select one backup you want to restore (e.g. xxx.tar.gz): " SELECT_BACKUP
>     # 起始时间
>     echo -e "${FONT_GREEN}Start restoring now..${FONT_SUFFIX}"
>     echo -e "[Starting time: `date +'%Y-%m-%d %H:%M:%S'`]"
>     TIME_START=$(date +%s)
>     # 登入FTP下载备份文件
>   ftp -i -n -v << END
>   open $FTP_IP
>   user $FTP_USER $FTP_PASSWD
>   binary
>   lcd $DIR_BACKUP
>   hash
>   prompt off
>   mget $SELECT_BACKUP
>   close
>   bye
>   END
>     # 删除临时文件
>     rm -rf nlist-ftp.tmp
>   else
>     echo -e "${FONT_RED}ERROR! Please select again!${FONT_SUFFIX}"
>     read -p "Please enter your choice/number (1 or 2): " SELECT_NUMBER
>   fi
>   
>   # 停止lnmp
>   lnmp stop
>   
>   # 解压所选备份文件并迁移所需数据
>   cd $DIR_BACKUP && tar -zxvf $SELECT_BACKUP
>   cd $(echo -e "$SELECT_BACKUP" | awk -F ".tar.gz" '{print $1}')
>   sed -i "s/SSLStrictSNIVHostCheck on/SSLStrictSNIVHostCheck off/g" /usr/local/apache/conf/extra/httpd-ssl.conf
>   sed -i "s/#Include conf\/extra\/httpd-ssl.conf/Include conf\/extra\/httpd-ssl.conf/g" /usr/local/apache/conf/httpd.conf
>   \cp -rdpv conf/vhost/* /usr/local/apache/conf/vhost/
>   \cp -rdpv conf/ssl /usr/local/apache/conf/
>   \cp -rdpv var/* /usr/local/mysql/var/
>   chown -R mysql:mysql /usr/local/mysql/var
>   \cp -rdpv wwwroot/* /home/wwwroot/
>   chmod -R 755 /home/wwwroot && chown -R www /home/wwwroot
>   \cp -rdpv acme.sh /usr/local/
>   \cp -rdpv /var/spool/cron/root /var/spool/cron/root.old
>   \cp -rdpv cron/* /var/spool/cron/
>   service crond restart
>   cd $DIR_BACKUP && rm -rf $(echo -e "$SELECT_BACKUP" | awk -F ".tar.gz" '{print $1}')
>   
>   # 重启lnmp
>   lnmp start && lnmp restart && lnmp restart
>   
>   # 计算耗时
>   echo -e "[End time: `date +'%Y-%m-%d %H:%M:%S'`]"
>   TIME_END=$(date +%s)
>   echo -e "${FONT_GREEN}[Successfully done! Command takes $((TIME_END-TIME_START)) seconds.]${FONT_SUFFIX}"
>   echo -e "$INFO If you get a problem, please leave a message at https://www.seoimo.com/wordpress-backup/"
>   
>   # 列出待选的已绑定域名及其对应的数据库信息
>   WP_DOMAIN=$(ls -F $DIR_WWWROOT | grep "/$" | awk -F "/" '{print $1}')
>   for wp in $WP_DOMAIN; do
>     if [ -f "$DIR_WWWROOT/$wp/wp-config.php" ]; then
>       DB_USER=$(grep "DB_USER" $DIR_WWWROOT/$wp/wp-config.php | awk -F "'" '{print $4}')
>       DB_PASSWORD=$(grep "DB_PASSWORD" $DIR_WWWROOT/$wp/wp-config.php | awk -F "'" '{print $4}')
>       printf "%-20s %-20s %-25s\n" $DB_USER $DB_PASSWORD $wp >> db-info-all-list.tmp
>     fi
>   done
>   # 筛选出需要修改的数据库信息
>   DB_NAME=$(awk '{print $1}' db-info-all-list.tmp)
>   for db in $DB_NAME; do
>     grep -a "$db" $DIR_DATABASE/mysql/user.MYI > db-info-sgl-list.tmp
>     if [ $? -ne 0 ]; then
>       grep "$db" db-info-all-list.tmp >> db-info-fix-list.tmp
>     fi
>   done
>   # 打印输出以便于后续操作
>   if [ -s "db-info-fix-list.tmp" ]; then
>     echo -e "--------------------------------------"
>     printf "%-20s %-20s %-25s\n" DB_NAME DB_PASSWORD DOMAIN
>     echo -e "${FONT_GREEN}$(cat db-info-fix-list.tmp)${FONT_SUFFIX}"
>     echo -e "Something missed out! Please add the database name(s) and database password(s) shown above. \nThe command is: ${FONT_GREEN}# lnmp database add ${FONT_SUFFIX} \nMaybe you should better copy and save the details before."
>     echo -e "--------------------------------------"
>   fi
>   # 删除临时文件
>   rm -rf db-info-*-list.tmp
>   
>   # 退出
>   exit 0
>   
>   ```
>
>   然后，执行脚本（选择从本地恢复还是从远程FTP恢复）：
>
>   \# bash /root/wp-restore-seoimo.sh
>
>    
>
>   \-------------------------------
>
>    
>
>   更多信息可查看原文：
>
>   https://www.seoimo.com/wordpress-backup/
>
>    

