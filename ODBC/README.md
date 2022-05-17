# ODBC를 활용한 DB Table Read

```cpp
#include <sql.h>
#include <sqlext.h>

int main()
{
	SQLHENV environment = SQL_NULL_HANDLE;
	SQLHDBC _connection = SQL_NULL_HANDLE;

	if(::SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &environment) != SQL_SUCCESS)
	  return 0;

	if(::SQLSetEnvAttr(environment, SQL_ATTR_ODBC_VERSION, reinterpret_cast<SQLPOINTER>(SQL_OV_ODBC3),0)!=SQL_SUCCESS)
	  return 0;

	if(::SQLAllocHandle(SQL_HANDLE_DBC, environment, &_connection)!=SQL_SUCCESS)
	  return 0;

	char stringBuffer[MAX_PATH] = "Driver = {SQL Server Native Client 11.0};UID=ID;PWD=pwd;SERVER=192.168.1.1;";
	char resultBuffer[MAX_PATH] = {0,};
  
	SQLSMALLINT resultLen = 0;
	SQLRETURN ret = ::SQLDriverConnectA(
		_connection,
		NULL,
		reinterpret_cast<SQLCHAR*>(stringBuffer),
		_countof(stringBuffer),
		OUT reinterpret_cast<SQLCHAR*>(resultBuffer),
		_countof(resultString),
		OUT & resultStringLen,
		SQL_DRIVER_NOPROMPT
	);
	
	SQLHSTMT _statement;
	if(::SQLAllocHandle(SQL_HANDLE_STMT, _connection, &_statement) != SQL_SUCCESS)
	   return 0;
	
	char EmpType[21] = {0,}, empNum[50] = {0,};
	SQLLEN empLen =0, empNumLen =0;
	SQLRETURN bindRet = ::SQLBindCol(_statement,1,SQL_C_CHAR,EmpType,sizeof(EmpType), &empLen);
	bindRet = ::SQLBindCol(_statement,2,SQL_C_CHAR,empNum,sizeof(empNum), &empNumLen);
	
	switch(bindRet)
	{
	      case SQL_SUCCESS:
		  break;
	      case SQL_SUCCESS_WITH_INFO:
	          break;
	      case SQL_NO_DATA:
	          break;
	      case SQL_ERROR:
	          break;
	}
	   
	char query[1024]  = "SELECT empNo, empNames FROM TABLE_Employee";
	if(ret == SQL_SUCCESS || ret == SQL_SUCCESS_WITH_INFO)
	{
	      if(::SQLExecDirecA(_statement, reinterpret_cast<SQLCHAR*>(query), SQL_NTSL) != SQL_SUCCESS)
	      	 return 0;
		 
     	      while(::SQLFetch(_statement) == SQL_SUCCESS || ::SQLFetch(_statement) == SQL_SUCCESS_WITH_INFO)
	      {
	      
	      }
	}
  
  return 0;
}
```
