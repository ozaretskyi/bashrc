#===============================================================
#
# ЛИЧНЫЙ ФАЙЛ $HOME/.bashrc для bash-2.05a (или выше)
#
# Время последней модификации: Втр Апр 15 20:32:34 CEST 2003
#
# Этот файл содержит настройки интерактивной командной оболочки.
# Здесь размещены определения псевдонимов, функций
# и других элементов Bash, таких как prompt (приглашение к вводу).
#
# Изначально, этот файл был создан в операционной системе Solaris,
# но позднее был переделан под Redhat
# --> Модифицирован под Linux.
# Большая часть кода, который находится здесь, была взята из
# Usenet (или Интернет).
# Этот файл содержит слишком много определений -- помните, это всего лишь пример.
#
#
#===============================================================

# --> Комментарии, добавленные автором HOWTO.
# --> И дополнены автором сценария Emmanuel Rouat :-)

#-----------------------------------
# Глобальные определения
#-----------------------------------

if [ -f /etc/bashrc ]; then
        . /etc/bashrc   # --> Прочитать настройки из /etc/bashrc, если таковой имеется.
fi

#-------------------------------------------------------------
# Настройка переменной $DISPLAY (если еще не установлена)
# Это срабатывает под linux - в вашем случае все может быть по другому....
# Проблема в том, что различные типы терминалов
# дают разные ответы на запрос 'who am i'......
# я не нашел 'универсального' метода
#-------------------------------------------------------------

function get_xserver ()
{
    case $TERM in
        xterm )
            XSERVER=$(who am i | awk '{print $NF}' | tr -d ')''(' )
            XSERVER=${XSERVER%%:*}
            ;;
        aterm | rxvt)
        # добавьте здесь свой код.....
            ;;
    esac
}

if [ -z ${DISPLAY:=""} ]; then
    get_xserver
    if [[ -z ${XSERVER}  || ${XSERVER} == $(hostname) || ${XSERVER} == "unix" ]]; then
        DISPLAY=":0.0"          # для локального хоста
    else
        DISPLAY=${XSERVER}:0.0  # для удаленного хоста
    fi
fi

export DISPLAY

#---------------
# Некоторые настройки
#---------------

ulimit -S -c 0          # Запрет на создание файлов coredump
set -o notify
set -o noclobber
set -o ignoreeof
set -o nounset
#set -o xtrace          # полезно для отладки

# Разрешающие настройки:
shopt -s cdspell
shopt -s cdable_vars
shopt -s checkhash
shopt -s checkwinsize
shopt -s mailwarn
shopt -s sourcepath
shopt -s no_empty_cmd_completion  # только для bash>=2.04
shopt -s cmdhist
shopt -s histappend histreedit histverify
shopt -s extglob

# Запрещающие настройки:
shopt -u mailwarn
unset MAILCHECK         # Я не желаю, чтобы командная оболочка сообщала мне о прибытии почты


export TIMEFORMAT=$'\nreal %3R\tuser %3U\tsys %3S\tpcpu %P\n'
export HISTIGNORE="&:bg:fg:ll:h"
export HOSTFILE=$HOME/.hosts    # Поместить список удаленных хостов в файл ~/.hosts



#-----------------------
# Greeting, motd etc...
#-----------------------

# Для начала определить некоторые цвета:
red='\e[0;31m'
RED='\e[1;31m'
blue='\e[0;34m'
BLUE='\e[1;34m'
cyan='\e[0;36m'
CYAN='\e[1;36m'
NC='\e[0m'              # No Color (нет цвета)
# --> Прекрасно. Имеет тот же эффект, что и "ansi.sys" в DOS.

# Лучше выглядит на черном фоне.....
echo -e "${CYAN}This is BASH ${RED}${BASH_VERSION%.*}${CYAN} - DISPLAY on ${RED}$DISPLAY${NC}\n"
date
if [ -x /usr/games/fortune ]; then
    /usr/games/fortune -s     # сделает наш день более интересным.... :-)
fi

function _exit()        # функция, запускающаяся при выходе из оболочки
{
    echo -e "${RED}Аста ла виста, бэби ${NC}"
}
trap _exit EXIT

#---------------
# Prompt
#---------------

if [[ "${DISPLAY#$HOST}" != ":0.0" &&  "${DISPLAY}" != ":0" ]]; then
    HILIT=${red}   # на удаленной системе: prompt будет частично красным
else
    HILIT=${cyan}  # на локальной системе: prompt будет частично циановым
fi

#  --> Замените \W на \w в функциях ниже
#+ --> чтобы видеть в оболочке полный путь к текущему каталогу.

function fastprompt()
{
    unset PROMPT_COMMAND
    case $TERM in
        *term | rxvt )
            PS1="${HILIT}[\h]$NC \W > \[\033]0;\${TERM} [\u@\h] \w\007\]" ;;
        linux )
            PS1="${HILIT}[\h]$NC \W > " ;;
        *)
            PS1="[\h] \W > " ;;
    esac
}

function powerprompt()
{
    _powerprompt()
    {
        LOAD=$(uptime|sed -e "s/.*: \([^,]*\).*/\1/" -e "s/ //g")
    }

    PROMPT_COMMAND=_powerprompt
    case $TERM in
        *term | rxvt  )
            PS1="${HILIT}[\A \$LOAD]$NC\n[\h \#] \W > \[\033]0;\${TERM} [\u@\h] \w\007\]" ;;
        linux )
            PS1="${HILIT}[\A - \$LOAD]$NC\n[\h \#] \w > " ;;
        * )
            PS1="[\A - \$LOAD]\n[\h \#] \w > " ;;
    esac
}

powerprompt     # это prompt по-умолчанию - может работать довольно медленно
                # Если это так, то используйте fastprompt....

#===============================================================
#
# ПСЕВДОНИМЫ И ФУНКЦИИ
#
# Возможно некоторые из функций, приведенных здесь, окажутся для вас слишком большими,
# но на моей рабочей станции установлено 512Mb ОЗУ, так что.....
# Если пожелаете уменьшить размер этого файла, то можете оформить эти функции
# в виде отдельных сценариев.
#
# Большинство функций были взяты, почти без переделки, из примеров
# к bash-2.04.
#
#===============================================================

#-------------------
# Псевдонимы
#-------------------

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
# -> Предотвращает случайное удаление файлов.
alias mkdir='mkdir -p'

alias h='history'
alias j='jobs -l'
alias r='rlogin'
alias which='type -all'
alias ..='cd ..'
alias path='echo -e ${PATH//:/\\n}'
alias print='/usr/bin/lp -o nobanner -d $LPDEST'   # Предполагается, что LPDEST определен
alias pjet='enscript -h -G -fCourier9 -d $LPDEST'  # Печать через enscript
alias background='xv -root -quit -max -rmode 5'    # Положить картинку в качестве фона
alias du='du -kh'
alias df='df -kTh'

# Различные варианты 'ls' (предполагается, что установлена GNU-версия ls)
alias la='ls -Al'               # показать скрытые файлы
alias ls='ls -hF --color'       # выделить различные типы файлов цветом
alias lx='ls -lXB'              # сортировка по расширению
alias lk='ls -lSr'              # сортировка по размеру
alias lc='ls -lcr'              # сортировка по времени изменения
alias lu='ls -lur'              # сортировка по времени последнего обращения
alias lr='ls -lR'               # рекурсивный обход подкаталогов
alias lt='ls -ltr'              # сортировка по дате
alias lm='ls -al |more'         # вывод через 'more'
alias tree='tree -Csu'          # альтернатива 'ls'

# подготовка 'less'
alias more='less'
export PAGER=less
export LESSCHARSET='latin1'
export LESSOPEN='|/usr/bin/lesspipe.sh %s 2>&-' # если существует lesspipe.sh
export LESS='-i -N -w  -z-4 -g -e -M -X -F -R -P%t?f%f \
:stdin .?pb%pb\%:?lbLine %lb:?bbByte %bb:-...'

# проверка правописания - настоятельно рекомендую :-)
alias xs='cd'
alias vf='cd'
alias moer='more'
alias moew='more'
alias kk='ll'

#----------------
# добавим немножко "приятностей"
#----------------

function xtitle ()
{
    case "$TERM" in
        *term | rxvt)
            echo -n -e "\033]0;$*\007" ;;
        *)
            ;;
    esac
}

# псевдонимы...
alias top='xtitle Processes on $HOST && top'
alias make='xtitle Making $(basename $PWD) ; make'
alias ncftp="xtitle ncFTP ; ncftp"

# .. и функции
function man ()
{
    for i ; do
        xtitle The $(basename $1|tr -d .[:digit:]) manual
        command man -F -a "$i"
    done
}

function ll(){ ls -l "$@"| egrep "^d" ; ls -lXB "$@" 2>&-| egrep -v "^d|total "; }
function te()  # "обертка" вокруг xemacs/gnuserv
{
    if [ "$(gnuclient -batch -eval t 2>&-)" == "t" ]; then
        gnuclient -q "$@";
    else
        ( xemacs "$@" &);
    fi
}

#-----------------------------------
# Функции для работы с файлами и строками:
#-----------------------------------

# Поиск файла по шаблону:
function ff() { find . -type f -iname '*'$*'*' -ls ; }
# Поиск файла по шаблону в $1 и запуск команды в $2 с ним:
function fe() { find . -type f -iname '*'$1'*' -exec "${2:-file}" {} \;  ; }
# поиск строки по файлам:
function fstr()
{
    OPTIND=1
    local case=""
    local usage="fstr: поиск строки в файлах.
Порядок использования: fstr [-i] \"шаблон\" [\"шаблон_имени_файла\"] "
    while getopts :it opt
    do
        case "$opt" in
        i) case="-i " ;;
        *) echo "$usage"; return;;
        esac
    done
    shift $(( $OPTIND - 1 ))
    if [ "$#" -lt 1 ]; then
        echo "$usage"
        return;
    fi
    local SMSO=$(tput smso)
    local RMSO=$(tput rmso)
    find . -type f -name "${2:-*}" -print0 | xargs -0 grep -sn ${case} "$1" 2>&- | \
sed "s/$1/${SMSO}\0${RMSO}/gI" | more
}

function cuttail() # удалить последние n строк в файле, по-умолчанию 10
{
    nlines=${2:-10}
    sed -n -e :a -e "1,${nlines}!{P;N;D;};N;ba" $1
}

function lowercase()  # перевести имя файла в нижний регистр
{
    for file ; do
        filename=${file##*/}
        case "$filename" in
        */*) dirname==${file%/*} ;;
        *) dirname=.;;
        esac
        nf=$(echo $filename | tr A-Z a-z)
        newname="${dirname}/${nf}"
        if [ "$nf" != "$filename" ]; then
            mv "$file" "$newname"
            echo "lowercase: $file --> $newname"
        else
            echo "lowercase: имя файла $file не было изменено."
        fi
    done
}

function swap()         # меняет 2 файла местами
{
    local TMPFILE=tmp.$$
    mv "$1" $TMPFILE
    mv "$2" "$1"
    mv $TMPFILE "$2"
}


#-----------------------------------
# Функции для работы с процессами/системой:
#-----------------------------------

function my_ps() { ps $@ -u $USER -o pid,%cpu,%mem,bsdtime,command ; }
function pp() { my_ps f | awk '!/awk/ && $0~var' var=${1:-".*"} ; }

# Эта функция является грубым аналогом 'killall' в linux
# но не эквивалентна (насколько я знаю) 'killall' в Solaris
function killps()   # "Прибить" процесс по его имени
{
    local pid pname sig="-TERM"   # сигнал, рассылаемый по-умолчанию
    if [ "$#" -lt 1 ] || [ "$#" -gt 2 ]; then
        echo "Порядок использования: killps [-SIGNAL] шаблон_имени_процесса"
        return;
    fi
    if [ $# = 2 ]; then sig=$1 ; fi
    for pid in $(my_ps| awk '!/awk/ && $0~pat { print $1 }' pat=${!#} ) ; do
        pname=$(my_ps | awk '$1~var { print $5 }' var=$pid )
        if ask "Послать сигнал $sig процессу $pid <$pname>?"
            then kill $sig $pid
        fi
    done
}

function my_ip() # IP адрес
{
    MY_IP=$(/sbin/ifconfig ppp0 | awk '/inet/ { print $2 } ' | sed -e s/addr://)
    MY_ISP=$(/sbin/ifconfig ppp0 | awk '/P-t-P/ { print $3 } ' | sed -e s/P-t-P://)
}

function ii()   # Дополнительные сведения о системе
{
    echo -e "\nВы находитесь на ${RED}$HOST"
    echo -e "\nДополнительная информация:$NC " ; uname -a
    echo -e "\n${RED}В системе работают пользователи:$NC " ; w -h
    echo -e "\n${RED}Дата:$NC " ; date
    echo -e "\n${RED}Время, прошедшее с момента последней перезагрузки :$NC " ; uptime
    echo -e "\n${RED}Память :$NC " ; free
    my_ip 2>&- ;
    echo -e "\n${RED}IP адрес:$NC" ; echo ${MY_IP:-"Соединение не установлено"}
    echo -e "\n${RED}Адрес провайдера (ISP):$NC" ; echo ${MY_ISP:-"Соединение не установлено"}
    echo
}

# Разные утилиты:

function repeat()       # повторить команду n раз
{
    local i max
    max=$1; shift;
    for ((i=1; i <= max ; i++)); do  # --> C-подобный синтаксис
        eval "$@";
    done
}

function ask()
{
    echo -n "$@" '[y/n] ' ; read ans
    case "$ans" in
        y*|Y*) return 0 ;;
        *) return 1 ;;
    esac
}

#=========================================================================
#
# ПРОГРАММНЫЕ ДОПОЛНЕНИЯ - ТОЛЬКО НАЧИНАЯ С ВЕРСИИ BASH-2.04
# Большая часть дополнений взята из докуентации к bash 2.05  и из
# пакета 'Bash completion' (http://www.caliban.org/bash/index.shtml#completion)
# автор -- Ian McDonalds
# Фактически, у вас должен стоять bash-2.05a
#
#=========================================================================

if [ "${BASH_VERSION%.*}" \< "2.05" ]; then
    echo "Вам необходимо обновиться до версии 2.05"
    return
fi

shopt -s extglob        # необходимо
set +o nounset          # иначе некоторые дополнения не будут работать

complete -A hostname   rsh rcp telnet rlogin r ftp ping disk
complete -A export     printenv
complete -A variable   export local readonly unset
complete -A enabled    builtin
complete -A alias      alias unalias
complete -A function   function
complete -A user       su mail finger

complete -A helptopic  help
complete -A shopt      shopt
complete -A stopped -P '%' bg
complete -A job -P '%'     fg jobs disown

complete -A directory  mkdir rmdir
complete -A directory   -o default cd

# Архивация
complete -f -o default -X '*.+(zip|ZIP)'  zip
complete -f -o default -X '!*.+(zip|ZIP)' unzip
complete -f -o default -X '*.+(z|Z)'      compress
complete -f -o default -X '!*.+(z|Z)'     uncompress
complete -f -o default -X '*.+(gz|GZ)'    gzip
complete -f -o default -X '!*.+(gz|GZ)'   gunzip
complete -f -o default -X '*.+(bz2|BZ2)'  bzip2
complete -f -o default -X '!*.+(bz2|BZ2)' bunzip2
# Postscript,pdf,dvi.....
complete -f -o default -X '!*.ps'  gs ghostview ps2pdf ps2ascii
complete -f -o default -X '!*.dvi' dvips dvipdf xdvi dviselect dvitype
complete -f -o default -X '!*.pdf' acroread pdf2ps
complete -f -o default -X '!*.+(pdf|ps)' gv
complete -f -o default -X '!*.texi*' makeinfo texi2dvi texi2html texi2pdf
complete -f -o default -X '!*.tex' tex latex slitex
complete -f -o default -X '!*.lyx' lyx
complete -f -o default -X '!*.+(htm*|HTM*)' lynx html2ps
# Multimedia
complete -f -o default -X '!*.+(jp*g|gif|xpm|png|bmp)' xv gimp
complete -f -o default -X '!*.+(mp3|MP3)' mpg123 mpg321
complete -f -o default -X '!*.+(ogg|OGG)' ogg123



complete -f -o default -X '!*.pl'  perl perl5

# Эти 'универсальные' дополнения работают тогда, когда команды вызываются
# с, так называемыми, 'длинными ключами', например: 'ls --all' вместо 'ls -a'

_get_longopts ()
{
    $1 --help | sed  -e '/--/!d' -e 's/.*--\([^[:space:].,]*\).*/--\1/'| \
grep ^"$2" |sort -u ;
}

_longopts_func ()
{
    case "${2:-*}" in
        -*)     ;;
        *)      return ;;
    esac

    case "$1" in
        \~*)    eval cmd="$1" ;;
        *)      cmd="$1" ;;
    esac
    COMPREPLY=( $(_get_longopts ${1} ${2} ) )
}
complete  -o default -F _longopts_func configure bash
complete  -o default -F _longopts_func wget id info a2ps ls recode


_make_targets ()
{
    local mdef makef gcmd cur prev i

    COMPREPLY=()
    cur=${COMP_WORDS[COMP_CWORD]}
    prev=${COMP_WORDS[COMP_CWORD-1]}

    # Если аргумент prev это -f, то вернуть возможные варианты имен файлов.
    # будем великодушны и вернем несколько вариантов
    # `makefile Makefile *.mk'
    case "$prev" in
        -*f)    COMPREPLY=( $(compgen -f $cur ) ); return 0;;
    esac

    # Если запрошены возможные ключи, то вернуть ключи posix
    case "$cur" in
        -)      COMPREPLY=(-e -f -i -k -n -p -q -r -S -s -t); return 0;;
    esac

    # попробовать передать make `makefile' перед тем как попробовать передать `Makefile'
    if [ -f makefile ]; then
        mdef=makefile
    elif [ -f Makefile ]; then
        mdef=Makefile
    else
        mdef=*.mk
    fi

    # прежде чем просмотреть "цели", убедиться, что имя makefile было задано
    # ключом -f
    for (( i=0; i < ${#COMP_WORDS[@]}; i++ )); do
        if [[ ${COMP_WORDS[i]} == -*f ]]; then
            eval makef=${COMP_WORDS[i+1]}
            break
        fi
    done

        [ -z "$makef" ] && makef=$mdef

    # Если задан шаблон поиска, то ограничиться
    # этим шаблоном
    if [ -n "$2" ]; then gcmd='grep "^$2"' ; else gcmd=cat ; fi

    # если мы не желаем использовать *.mk, то необходимо убрать cat и использовать
    # test -f $makef с перенаправлением ввода
    COMPREPLY=( $(cat $makef 2>/dev/null | awk 'BEGIN {FS=":"} /^[^.#   ][^=]*:/ {print $1}' | tr -s ' ' '\012' | sort -u | eval $gcmd ) )
}

complete -F _make_targets -X '+($*|*.[cho])' make gmake pmake


# cvs(1) completion
_cvs ()
{
    local cur prev
    COMPREPLY=()
    cur=${COMP_WORDS[COMP_CWORD]}
    prev=${COMP_WORDS[COMP_CWORD-1]}

    if [ $COMP_CWORD -eq 1 ] || [ "${prev:0:1}" = "-" ]; then
        COMPREPLY=( $( compgen -W 'add admin checkout commit diff \
        export history import log rdiff release remove rtag status \
        tag update' $cur ))
    else
        COMPREPLY=( $( compgen -f $cur ))
    fi
    return 0
}
complete -F _cvs cvs

_killall ()
{
    local cur prev
    COMPREPLY=()
    cur=${COMP_WORDS[COMP_CWORD]}

    # получить список процессов
    COMPREPLY=( $( /usr/bin/ps -u $USER -o comm  | \
        sed -e '1,1d' -e 's#[]\[]##g' -e 's#^.*/##'| \
        awk '{if ($0 ~ /^'$cur'/) print $0}' ))

    return 0
}

complete -F _killall killall killps


# Функция обработки мета-команд
# В настоящее время недостаточно отказоустойчива (например, mount и umount
# обрабатываются некорректно), но все еще актуальна. Автор Ian McDonald, изменена мной.

_my_command()
{
    local cur func cline cspec

    COMPREPLY=()
    cur=${COMP_WORDS[COMP_CWORD]}

    if [ $COMP_CWORD = 1 ]; then
        COMPREPLY=( $( compgen -c $cur ) )
    elif complete -p ${COMP_WORDS[1]} &>/dev/null; then
        cspec=$( complete -p ${COMP_WORDS[1]} )
        if [ "${cspec%%-F *}" != "${cspec}" ]; then
            # complete -F <function>
            #
            # COMP_CWORD and COMP_WORDS() доступны на запись,
            # так что мы можем установить их перед тем,
            # как передать их дальше

            # уменьшить на 1 текущий номер лексемы
            COMP_CWORD=$(( $COMP_CWORD - 1 ))
            # получить имя функции
            func=${cspec#*-F }
            func=${func%% *}
            # получить командную строку, исключив первую команду
            cline="${COMP_LINE#$1 }"
            # разбить на лексемы и поместить в массив
                COMP_WORDS=( $cline )
            $func $cline
        elif [ "${cspec#*-[abcdefgjkvu]}" != "" ]; then
            # complete -[abcdefgjkvu]
            #func=$( echo $cspec | sed -e 's/^.*\(-[abcdefgjkvu]\).*$/\1/' )
            func=$( echo $cspec | sed -e 's/^complete//' -e 's/[^ ]*$//' )
            COMPREPLY=( $( eval compgen $func $cur ) )
        elif [ "${cspec#*-A}" != "$cspec" ]; then
            # complete -A <type>
            func=${cspec#*-A }
        func=${func%% *}
        COMPREPLY=( $( compgen -A $func $cur ) )
        fi
    else
        COMPREPLY=( $( compgen -f $cur ) )
    fi
}


complete -o default -F _my_command nohup exec eval trace truss strace sotruss gdb
complete -o default -F _my_command command type which man nice

# Локальные переменные:
# mode:shell-script
# sh-shell:bash
# Конец:
