## Oracle提示错误消息ORA-28001: the password has expired

select * from dba_profiles 
where profile = 'DEFAULT' 
and RESOURCE_type = 'PASSWORD'

--去除180天的密码生存周期的限制
alter profile default limit password_life_time unlimited;

--进行以上步骤之后需要改变密码，否则还会出现password has expired异常
-- 改变密码的命令
alter user XXXUSER identified by Welcome1; 

--如果账号被锁住，则需要解锁命令
alter user XXXUSER identified by oracle account unlock; 

--再次调试，问题解决



## --登录忽略大小写
alter system set sec_case_sensitive_logon=false

