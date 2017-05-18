## AWS 서버세팅

### 1. AWS EC2 instance 생성
---

### 2. AWS EC2에 SSH로 접속
---


### 3. 개발환경설정

```
sudo apt-get update
```

```
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs
```

```
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 2.4.0
rbenv global 2.4.0
ruby -v
```

```
gem install bundler
```

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add Passenger APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger & Nginx
sudo apt-get install -y nginx-extras passenger
```

```
sudo service nginx start
```

```
sudo vi /etc/nginx/nginx.conf
```

```
##
# Phusion Passenger
##
# Uncomment it if you installed ruby-passenger or ruby-passenger-enterprise
##

include /etc/nginx/passenger.conf;
```

```
sudo vi /etc/nginx/passenger.conf
```

```
passenger_ruby /home/deploy/.rbenv/shims/ruby; # If you use rbenv
```

```
sudo service nginx restart
```

```
sudo vi /etc/nginx/sites-enabled/default
```

```
server {
        listen 80;
        listen [::]:80 ipv6only=on;

        server_name 내_도메인.com; #자신의 domain을 넣으세요!
        passenger_enabled on;
        rails_env    production;
        root         /home/ubuntu/나의_app_이름/current/public; #자신의 app이름을 넣으세요 !

        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}
```