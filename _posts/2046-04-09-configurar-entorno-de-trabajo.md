## Janus VIM
sudo pacman -S gvim ctags ruby ack
curl -L https://bit.ly/janus-bootstrap | bash

## Rails
### Ruby rvm
#echo "source ~/.profile" >> ~/.bash_profile
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
curl -L https://get.rvm.io | bash -s stable --autolibs=enabled --auto-dotfiles
rvm install ruby-2.2.0
rvm --default use ruby-2.2.0
### gemas
gem install bundler graphviz

## Ionic
### NodeJS rvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
nvm install 4.0
nvm use 4.0
### tools
sudo chown -R xx.xx .nvm/
npm install -g bower gulp node-sass cordova ionic

## Heroku
wget -O- https://toolbelt.heroku.com/install.sh | sh
echo 'PATH="/usr/local/heroku/bin:$PATH"' >> ~/.profile

## WebDev Tools
npm install -g grunt-cli
gem install compass


gem install foreman




rvm pkg install openssl
rvm list
rvm reinstall all --force
rvm install ruby-2.2.0 --with-openssl-dir=$HOME/.rvm/usr
rvm --default use 2.2.0

# Editar gruntfile para tener en cuenta todos los subdirectorios de vistas
    ngtemplates: {
      dist: {
        options: {
          module: 'app',
          htmlmin: '<%= htmlmin.dist.options %>',
          usemin: 'scripts/scripts.js'
        },
        cwd: '<%= yeoman.app %>',
        src: 'views/**/*.html',
        dest: '.tmp/templateCache.js'
      }
    },
