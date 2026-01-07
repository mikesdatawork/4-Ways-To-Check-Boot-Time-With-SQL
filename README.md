![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# 4 Ways To Check Boot Time With SQL
**Post Date: November 10, 2017**

![Check Boot Time With SQL]( https://mikesdatawork.files.wordpress.com/2017/11/image0012.png "SQL Boot Time")


## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>4 Basic ways to check boot and service start times<p>
  
  
## SQL-Logic
```SQL
use master;
set nocount on
 
declare 
  @last_boot table 
  (
      [os_boot]             datetime
    , [first_session]       datetime
    , [default_trace_start] datetime
    , [tempdb_created]      datetime
   )
insert into @last_boot
select
    (select sqlserver_start_time from sys.dm_os_sys_info)
,   (select login_time from sys.dm_exec_sessions where session_id = 1)
,   (select start_time from sys.traces where is_default = 1)
,   (select create_date from sys.databases where name = 'tempdb')
 
select
    'boot_time'             = left([os_boot], 19)
,   'days_since_boot'       = datediff(day, [os_boot], getdate())
,   'sql__start'            = left([tempdb_created], 19)
,   'days_since_sql_start'  = datediff(day, [os_boot], getdate())
,   'default_trace_start'   = left([default_trace_start], 19)
,   'first_session'         = left([first_session], 19)
from
    @last_boot
```

Often times you'll need to check server or database server start times. In this example; there is a quick script that will show you 4 valid types of date types that can be returned to detect the start time both of sql server, and the operating system. Here you can easily see that the database service was last restarted at the time when the server was rebooted.


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

