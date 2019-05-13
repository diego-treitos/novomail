# novomail
Checks for new mail in a maildir group of directories

# usage

```
usage: ./novomail [-h] [-v] [-V] -a ACCOUNT [-t TEMPLATE] [-f MINUTES]
                  [-x COMMAND] [-X EXEC_TEMPLATE]

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Show the structure of the dict to be used in the
                        Jinja2 template and some other debug information.
                        Supress other output
  -V, --version         print version and exit
  -a ACCOUNT, --account ACCOUNT
                        Id and path to a directory containing the maildirs of
                        one account. (i.e. account1:~/.mutt/account1/) It can
                        be used several times for multiple accounts.
  -t TEMPLATE, --template TEMPLATE
                        Jinja2 template receiving the results dict as render
                        context. (default: {% for a in accounts %}{% if
                        a.new_count > 0 %}{{ a.name }}: {{ a.new_count }} {%
                        endif %}{% endfor %})
  -f MINUTES, --forget MINUTES
                        Number of minutes after which the new mails are not
                        anymore counted as new. (default: 300)
  -x COMMAND, --exec COMMAND
                        Execute command when new mails are detected in any of
                        the accounts. Before executing the command and its
                        arguments, the @T@ sequence will be replaced with the
                        result of the -X parameter.
  -X EXEC_TEMPLATE, --exec-template EXEC_TEMPLATE
                        Jinja2 template receiving the results dict as render
                        context that will be used in the -x command, replacing
                        the @T@ string.
```

# examples

```
novomail -a account1:~/.mutt/account1/ -a account2:~/.mutt/account2/ \
   -t '{% if total_new > 0 %}New mails: {{ total_new }}{% else %}No new mail{% endif %}' \
   -x 'notify-send "New Mail" "@T@"' \
   -X '{% for a in accounts %}{{ a.name }}: {{ a.new_count }}
{% endfor %}' \
   -f 30
```

