```bash
#!/bin/bash
function diskspace {
	clear
	df -k
}
function whoseon {
	clear
	who
}
function memusage {
	clear
	cat /proc/meminfo
}
function menu {
	clear
	echo
	echo -e "\t\t\tSys Admin Menu\n"
	echo -e "\t1.Display disk space"
	echo -e "\t2.Display logged on users"
	echo -e "\t3.Display memory usage"
	echo -e "\t0.Exit menu\n\n"
	echo -en "\t\tEnter option: "
	read -n 1 option
}

while [[ 1 ]]; do
	menu
	case $option in
		0)	break;;
		1)	diskspace;;
		2)	whoseon;;
		3)	memusage;;
		*)	clear
			echo "Sorry,wrong selection";;
		esac
	echo -en "\n\n\t\t\tHit any key to continue"
	read -n 1 line
done
clear
```

```bash
#!/bin/bash
function diskspace {
	clear
	df -k
}
function whoseon {
	clear
	who
}
function memusage {
	clear
	cat /proc/meminfo
}

PS3="Enter option: "
select option in "Display disk space" "Display logged on users" "Display memory usage" "Exit program"
do
	case $option in
		"Exit program" )	break;;
		"Display disk space")	diskspace;;
		"Display logged on users")	whoseon;;
		"Display memory usage")	meminfo;;
		*)	clear 
			echo "Sorry, wrong selection";;
	esac
done
clear
```

`sudo apt-get install dialog`

```bash
#!/bin/bash
temp=$(mktemp -t test.XXXXX)
temp2=$(mktemp -t test2.XXXXX)
function diskspace {
	df -k > $temp
	dialog --textbox $temp 20 60
}
function whoseon {
	who > $temp
	dialog --textbox $temp 20 50
}
function memusage {
	ps -fa >$temp
	dialog --textbox $temp 20 50
}

while [ 1 ]; do
    dialog --menu "Sys Admin Menu" 20 30 10 1 "Display disk space" 2 "Display users" 3 "Display memory usage" 0 "Exit" 2> $temp2
    if [ $? -eq 1 ]; then
    	break
    fi
    selection=$(cat $temp2)
    case $selection in
    	1)	diskspace ;;
    	2)	whoseon ;; 
		3)	memusage ;;
    	0)	break ;; 
		*)	dialog --msgbox "Sorry, invalid selection" 10 30
    esac
done
rm -f $temp 2> /dev/null
rm -f $temp2 2> /dev/null
```

[下一篇](shell8.md)

