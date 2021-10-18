# SQL

## SQL BACKUP DATABASE 문

  - `BACKUP DATABASE` 문은 SQL Server에서 기존 SQL 데이터베이스의 전체 백업을 만드는 데 사용된다.

    > **BACKUP DATABASE** databasename
    >
    > **TO DIST** = 'filepath';

  - **차등 백업** 은 마지막 전체 데이터베이스 백업 이후 **변경된 데이터베이스 부분만 백업** 한다.

    > **BACKUP DATABASE** databasename
    > **TO DISK** = '*filepath*'
    > **WITH DIFFERENTIAL**;

  - 다음 SQL 문은 기존 데이터베이스 `"testDB"`의 전체 백업을 D 디스크에 생성한다.

    > **BACKUP DATABASE** testDB
    >
    > **TO DISK** = 'D:\backups\testDB.bak';

  - ( 팁 )  항상 실제 데이터베이스와 다른 드라이브에 데이터베이스를 백업하자. 그러면 디스크 충돌이 발생해도 데이터베이스와 함께 백업 파일이 손실되지 않는다.

  - 다음 SQL 문은 `"testDB"` 데이터베이스의 차등 백업을 생성한다.

    > **BACKUP DATABASE** testDB
    >
    > **TO DISK** = 'D:\backups\testDB.bak'
    >
    > **WITH DIFFERENTIAL**;

  - ( 팁 )  차등 백업은 백업 시간을 줄인다 (변경 사항만 백업되기 때문에).
  
  
# References

- [SQL Backup DB - w3schools.com](https://www.w3schools.com/sql/sql_backup_db.asp) 

