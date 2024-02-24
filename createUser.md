<!-- change dir ownership -->
sudo chown "$USER":"$USER" docker-data/ -R

<!-- com create user -->
./setup.sh email add info@9hs.my.id password
<!-- or this com -->
docker run --rm -e MAIL_USER=info@9hs.my.id -e MAIL_PASS=samaja1110 -it mailserver/docker-mailserver /bin/sh -c 'echo "$MAIL_USER|$(doveadm pw -s SHA512-CRYPT -u $MAIL_USER -p $MAIL_PASS)"' >> ./docker-data/dms/config/postfix-accounts.cf

<!-- create DKIM privKey -->
./setup.sh config dkim

