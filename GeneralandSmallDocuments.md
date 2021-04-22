# General and Small Documents

- [using jupiter Notebook in Django](#using-jupiter-notebook-in-django)
- [Generate and Add SSH key to github](#Generate-and-Add-SSH-key-to-github)
- [2nd keyboard language in raspberry pi](#2nd-keyboard-Language-in-rasbpberry-pi)
- [Install Oh my zsh](#Install-oh-my-zsh)

### using Jupiter Notebook in django
install This requirements before:
```
$ pip install jupyter ipython django-extensions
```

add django_extensions to Settings:
```
INSTALLED_APPS = [
    ...
    'django_extensions',
]
```
and u can run notebook by this Command:
```
$ python manage.py shell_plus --notebook
```

One more step; u need to sync djanog orm with jupiter:
```
os.environ['DJANGO_ALLOW_ASYNC_UNSAFE']='true'
```
if you don't add this, you can't be able to access to database.


### Generate and Add SSH key to github

You should go for this page

[add ssh key to github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

and you should know that you have to clone repo with ssh and after that you can use ssh advantages

### 2nd keyboard Language in rasbpberry pi

sudo nano /home/pi/.config/lxpanel/LXDE-pi/panels/panel
At the end of the file change to
```
Plugin {
type=xkb
Config {
Model=pc105
LayoutsList=us,ru
VariantsList=,
ToggleOpt=grp:alt_shift_toggle
KeepSysLayouts=0
DisplayType=0
}
}
```

### Install oh-my zsh

1. install zsh:
```
sudo apt install zsh
```
2. download and install oh-my zsh:
```
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```


