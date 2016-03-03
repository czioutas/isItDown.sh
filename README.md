# isItDown.sh

A small bash script that outputs any state other than 200 for a specific server.

```

read -r -p "Please enter the server you want to monitor : " c

echo "Pinging server for response " $c
echo "Press [CTRL+C] to stop.."

authentication=''
if [[ $c =~ .*qa.* ]]
then
   	authentication=https://username:password
   	c=$authentication$c
  	echo "Using authentication for QA system: " $c
else
	headers=$'--header \'X-Forwarded-Proto: https\' --header \'Host: www.exmpleHost.com\''
fi

while :
do
	response=$(echo curl --write-out %{http_code} $headers --silent --output /dev/null $c | bash)
	
	if [ $response -ne 200 ]; then
		echo "$(date)" $response
	fi
	sleep 1
done
```
