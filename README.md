# psfiles

        
        ##############Uptime##############

        
        remove-item c:\temp\at\uptime.txt -Force
        
        $names = Get-Content C:\temp\at\testpatchservers.txt
        #$names = get-content C:\temp\at\Input\servers-prod-20july17.txt
        


        @( 
           foreach ($name in $names) 
          { 
            if ( Test-Connection -ComputerName $name -Count 1 -ErrorAction SilentlyContinue )  
          { 
            $wmi = gwmi Win32_OperatingSystem -computer $name 
            $LBTime = $wmi.ConvertToDateTime($wmi.Lastbootuptime) 
            [TimeSpan]$uptime = New-TimeSpan $LBTime $(get-date) 
            Write-output "$name Uptime is  $($uptime.days) Days $($uptime.hours) Hours $($uptime.minutes) Minutes $($uptime.seconds) Seconds" 
          } 
             else { 
                Write-output "$name is not reachable" 
                  } 
               } 
         ) | Out-file -FilePath "C:\temp\at\uptime.txt"


 Send-MailMessage -From patchservers@domain.com -to admin@domain.com -Subject "Ping patched servers" -Attachments C:\temp\at\uptime.txt -SmtpServer smtp.domain.com
 

