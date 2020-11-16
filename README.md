# Sending-a-specific-log-file-with-Rsyslog

<b>Receiver</b>

vi /etc/rsyslog.d/50-default.conf

	$template TEMPLATE_NAME_RECEIVER,"/var/log/TEMPLATE_NAME_RECEIVER-%$YEAR%%$MONTH%%$DAY%%$HOUR%.log"
	if($fromhost-ip == "SENDER_IP_ADDRESS") then ?TEMPLATE_NAME_RECEIVER
	& stop

<b>Sender</b>

vi /etc/rsyslog.d/ANY_TEMPLATE_NAME.conf

	$ModLoad imfile
	$InputFilePollInterval 10
	$InputFileName  /var/log/NAME_OF_YOUR_FILE.log
	$InputFileTag ANY_TAG:
	$InputRunFileMonitor
	
	$template ANY_TEMPLATE_NAME, "%hostname% %programname% %msg%"
	
	if $programname == 'ANY_TAG' then @RECIPIENT_IP_ADDRESS:514;ANY_TEMPLATE_NAME
	if $programname == 'ANY_TAG' then stop
