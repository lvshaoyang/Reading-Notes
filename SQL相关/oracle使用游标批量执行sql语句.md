数据库版本：Oracle 11g


--新建用户
CREATE USER KEYAN_710KJ_DM IDENTIFIED BY KEYAN_710KJ_DM DEFAULT TABLESPACE RDSYSEDUV710710KJ QUOTA UNLIMITED ON RDSYSEDUV710710KJ;
grant create session to KEYAN_710KJ_DM;

--执行一下pl/sql
begin
  ---定义游标
 declare cursor role_cur is select table_name as tname from user_tables where table_name like 'DM%' AND TABLESPACE_NAME='RDSYSEDUV710710KJ' 
 and table_name not in(select table_name from sys.all_synonyms where table_name like 'DM%' AND TABLE_OWNER LIKE 'RDSYSEDUV710710KJ');
 vs_sql varchar2(100) := '';syn_sql varchar2(256) :='';
  begin
    ---循环
    for roles in role_cur 
      loop
        vs_sql :='';
        syn_sql :='';
        --vs_sql := 'grant select on ' || roles.tname || ' to KEYAN_710KJ_DM  WITH GRANT OPTION';
        --execute immediate vs_sql;
        syn_sql := 'create public synonym ' || roles.tname || ' for RDSYSEDUV710710KJ.' || roles.tname;
        dbms_output.put_line(roles.tname);
        execute immediate syn_sql;
      end loop;
  end;
end;


方法1：

根据指定用户名获得对应用户所拥有权限的表

SELECT table_name, owner FROM all_tables WHERE owner = 'SCOTT';搜索

方法2：
 
通过tab视图获得当前登录用户所有表和视图，通过tabletype过滤获得所有表

SELECT * FROM tab WHERE tabtype = 'TABLE';
 
方法3：
 
根据user_tables表获得当前用户拥有所有表

SELECT table_name FROM user_tables;

方法4：
 
根据sys表空间下all_object表获得指定用户指定类型对象（表）

SELECT object_name FROM sys.all_objects WHERE owner='SCOTT' AND object_type='TABLE';