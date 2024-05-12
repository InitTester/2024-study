![](https://github.com/InitTester/2024-study/assets/148026641/7088a4f5-c0ba-4926-9b43-35769ae5e052)

# [5회차 과제] 
## 기존 Dao SQL문 prepared statement 형식으로 변경
## MemberDao
### 기존코드
```java
public int join(HashMap<String, String> params) {
    String sql = "INSERT INTO forum.`member` "
            + "(member_id, passwd, member_nm, email, auth_yn, pwd_chng_dtm, join_dtm) "
            + "VALUES('"+ params.get("memberId")
            + "', '" + params.get("passwd")
            + "', '', '" + params.get("email")
            + "', '', '', DATE_FORMAT(NOW()  ,'%Y%m%d%H%i%s')) ";

    return update(sql);  
}	
```
### 변경코드
```java
	public int join(HashMap<String, String> params) {
		String sql = "INSERT INTO forum.`member` "
				+ "(member_id, passwd, member_nm, email, auth_yn, pwd_chng_dtm, join_dtm) "
				+ "VALUES( ?, ?, '', ?, '', '', DATE_FORMAT(NOW()  ,'%Y%m%d%H%i%s')) ";

		Object[] args = {params.get("memberId"), params.get("passwd"), params.get("email")};

		return update(sql, args); 
	}	
```
기존 Dao SQL문 prepared statement(?) 형식으로 변경

## MemberAuthDao
### 기존코드
```java
public int addAuthInfo(MemberAuthDto memberAuthDto) {	
    String sql ="INSERT INTO forum.member_auth "
            + "(member_seq, auth_num, auth_uri, reg_dtm, expire_dtm, auth_yn) "
            + "VALUES( '" + memberAuthDto.getMemberSeq()
            + "', '', '" + memberAuthDto.getAuthUri()
            + "', DATE_FORMAT(NOW()  ,'%Y%m%d%H%i%s'), "
            + "'" + memberAuthDto.getExpireDtm()
            + "', 'N'); ";
    
    return update(sql);
}
```
### 수정코드
```java
public int addAuthInfo(MemberAuthDto memberAuthDto) {	
    String sql ="INSERT INTO forum.member_auth "
            + "(member_seq, auth_num, auth_uri, reg_dtm, expire_dtm, auth_yn) "
            + "VALUES( ?, '', ?, DATE_FORMAT(NOW()  ,'%Y%m%d%H%i%s'), ?, 'N'); ";
    Object[] args = {memberAuthDto.getMemberSeq(), memberAuthDto.getAuthUri(), memberAuthDto.getExpireDtm()};
    
    return update(sql, args);
}
```
### 기존코드
```java
	public MemberAuthDto getMemberAuthDto(String uri) {		
	    String sql ="SELECT auth_seq, member_seq, auth_num, auth_uri, reg_dtm, expire_dtm, auth_yn "
	            + "FROM forum.member_auth "
	            + "WHERE auth_uri ='"+uri+"' AND auth_yn='N' ";
	    return query(sql, new MemberAuthRowMapper());
	}
```
### 수정코드
```java
public MemberAuthDto getMemberAuthDto(String uri) {		
    String sql ="SELECT auth_seq, member_seq, auth_num, auth_uri, reg_dtm, expire_dtm, auth_yn "
            + "FROM forum.member_auth "
            + "WHERE auth_uri =? AND auth_yn='N' ";
    Object[] args = {uri};
    return query(sql, new MemberAuthRowMapper(), args);
}
```

### 기존코드
```java
public int updateAuthInfo(MemberAuthDto memberAuthDto) {
    String sql = "UPDATE forum.member_auth  "
            + "SET auth_yn = 'Y' "
            + "WHERE member_seq=" + memberAuthDto.getMemberSeq();

    return update(sql);
}
```

### 수정코드
```java
public int updateAuthInfo(MemberAuthDto memberAuthDto) {
    String sql = "UPDATE forum.member_auth  "
            + "SET auth_yn = 'Y' "
            + "WHERE member_seq= ? " ;
    Object[] args = {memberAuthDto.getMemberSeq()};

    return update(sql, args);
}
```
