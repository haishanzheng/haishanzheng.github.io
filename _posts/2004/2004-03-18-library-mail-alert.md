---
title:  "厦大图书馆图书到期mail提醒linux单机简约版"
date:   2004-03-18 21:50:36 +0800
---

源于2004年01月01日在鼓浪听涛图书馆版的这个 http://bbs.xmu.edu.cn/bbscon?board=XMU_Library&file=M.1073131548.A 讨论，好像ganr已经在找人做了，不知道进展如何。今天吃饱了花了几个小时做了一个简约版本，这个版本适合自己有linux的机器，必须有perl和一些必要的module，首先修改最开始的变量，设置自己的用户名，图书证号，密码，电子邮件，把脚本加入crontab，每天4点运行，如果有书快到期了，脚本会自动给你的mail发一封信。如果你有兴趣，可以重新包装我的这个脚本，做成多用户，让用户可以注册可以修改数据，把用户数据保存入mysql，每天4点开始更新一下数据，完了给所有将要过期的人发mail，好像也不是太难咯:)  

源代码请点击详细信息查看，本代码只提供一种思路，你可以任意修改，本人概不对使用本程序后造成图书丢失，超期，CPU烧毁，妻离子散等事负责。  

    #!/usr/bin/perl  

    #Written by Haishion @ xmubbs  

    #blog: https://dog.xmu.edu.cn  

    #email: haishanzheng AT sina DOT com  

    use strict 'vars';  

    #在这里定义变量，改变成你自己的用户名和密码  

    my $v_regname = "RegName";  

    my $v_cardno = "";  

    my $v_passwd  = "Password";  

    my $alarm_day = 7;							#需要几天开始提醒你？  

    my $mailprog = "/usr/lib/sendmail";			#email发送程序的位置  

    my $youremail = "xxx\@xxx.com";		#发送给的email地址，注意 @ 前面有一个 \  

    #==============================除非你知道你在干吗，不要修改下面的语句====================  

    use LWP::UserAgent;  

    use HTTP::Request;  

    use HTTP::Response;  

    my ($myurl, $ua, $request, $response);  

    my ($content);  

    my ($line);  

    my ($w_rdrecno, $v_rdrecno);  

    my ($book_title, $return_date);  

    $ua = new LWP::UserAgent;			  

    $myurl = "http://210.34.4.10/cgi-bin/confirmuser?v_newuser=0&v;_cardno=$v_cardno&v;_regname=$v_regname&v;_passwd=$v_passwd";  

    $request = new HTTP::Request('GET', $myurl);  

    $response = $ua->request($request);			  

    foreach $line (split(/\n/, $response->content)) {  

    	if ($line =~ m|<INPUT TYPE=hidden name=v_regname value=(.*?)>|) {  

    		$v_regname = $1;  

    	}  

    	if ($line =~ m|<INPUT TYPE=hidden name=w_rdrecno value=(.*?)>|) {  

    		$w_rdrecno = $1;  

    	}  

    	if ($line =~ m|<INPUT TYPE=hidden name=v_cardno value=(.*?)>|) {  

    		$v_cardno = $1;  

    	}  

    	if ($line =~ m|<INPUT TYPE=hidden name=v_rdrecno value=(.*?)>|) {  

    		$v_rdrecno = $1;  

    	}  

    }  

    my $alarm_date = time();  

    $alarm_date += $alarm_day * 60 * 60 * 24;  

    my ($sec, $min, $hour, $mday, $mon, $year, $wday, $ydat, $isdst) = localtime($alarm_date);  

    $year += 1900;  

    $mon++;  

    $alarm_date = sprintf("%04d%02d%02d", $year, $mon, $mday);  

    $myurl = "http://210.34.4.10/cgi-bin/SrchLoan?v_regname=$v_regname&w;_rdrecno=$w_rdrecno&v;_cardno=$v_cardno&v;_rdrecno=$v_rdrecno";  

    $request->url($myurl);  

    $response = $ua->request($request);			  

    $content = $response->content;  

    $content =~ s/\s+/ /g;  

    $content =~ s/<\/tr>/<\/tr>\n/g;  

    foreach $line (split(/\n/, $content)) {  

    		if ($line =~ m|<tr>\s*<td .*?><font .*?>题名</font></td>\s*<td .*?>(.*?)</td>\s*<td .*?><font .*?>责任者</font></td>\s*<td .*?>(.*?)</td>\s*</tr>|) {  

    #			print "\n\n题名$1 责任者$2\n";  

    			$book_title = $1;  

    			next;  

    		}  

    		if ($line =~ m|<tr>\s*<td .*?><font .*?>索取号</font></td>\s*<td .*?>(.*?)</td>\s*<td .*?><font .*?>条码号</font></td>\s*<td .*?>(.*?)</td>\s*<td .*?><font .*?>馆藏地点</font></td>\s*<td .*?>(.*?)</td>\s*<td .*?><font .*?>流通类型</font></td>\s*<td .*?>(.*?)</td>\s*</tr>|) {  

    #			print "索取号$1 条码号$2\n";  

    			next;  

    		}  

    		if ($line =~ m|<td .*?><font .*?>借出日期</font></td>\s*<td .*?>(.*?)</td><td .*?><font .*?>应还日期</font></td>\s*<td .*?>(.*?)</td>|) {  

    			$return_date = $2;  

    #			print "借出日期$1 应还日期$2\n\n\n";  

    #			print "$alarm_date $return_date\n\n";  

    			if ($alarm_date >= $return_date) {  

    				print "$book_title 这本书你得还了。。。\n";  

    				mailto($youremail, $youremail, "$book_title 这本书你得还了。。。\n", "$book_title 这本书你得还了。。。\n");  

    			}  

    			next;  

    		}  

    }  

    sub mailto() {  

    	my ($fromemail, $toemail, $title, $content) = @_;  

    	open(MAIL, "|$mailprog -f $toemail -t") or die;   

    	# Create headers   

    	print MAIL "From: $fromemail\n";  

    	print MAIL "To: $toemail\n";  

    	print MAIL "Subject: $title\n";  

    	print MAIL "\n\n$content\n";  

    	close(MAIL);   

    }  

