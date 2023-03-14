# MySQL
- log-bin
	- mysql에서 바이너리 로그는 쿼리 수행을 로그로 남기는 것이다. 이는 로그 백업으로 사용되어 복구하는데 쓰일 수도 있고 Replication 사용시 동기화에도 사용된다.
	- my.cnf 에 다음과 같은 파라미터들을 설정할 수 있다.
		- log-bin: 바이너리 로그 경로
		- binlog_cache_size: 바이너리 로그 캐시 사이즈, 기본값 18446744073709547520bytes, 추천되는 값은 4gb(4294967296) MySQL이 현재 4GB보다 큰 binary log 로는 작업할 수 없기 때문
		- max_binlog_size: 바이너리 로그 최대 사이즈. 기본값 1073741824
		- binlog_expire_logs_seconds: 보관 기간, 기본값 2582000(30일)