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
  
  return 0;
}
```
