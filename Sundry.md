# Sundry


#### Github codespaces

 - Telstra ISP was blocking ability to access port forwarded URL.  This worked fine when hotspotting to Optus mobile.  Changed to Google DNS settings using following instructions:   https://developers.google.com/speed/public-dns/docs/using
<br><br>
#### To adjust all JPG image dates 
...by adding 1 year, 12 month, 28 days, 14 hours, 54 minutes, 32 seconds <br>
https://photo.stackexchange.com/questions/7919/how-to-shift-exif-date-time-created-by-time-in-days-hours-minutes

exiftool "-AllDates+=1:12:28 14:54:32" -verbose *.jpg
