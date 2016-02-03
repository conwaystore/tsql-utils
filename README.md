# SPMakeListOfExpressions
> Retorna una lista de valores, envuelva entre comillas simples, separados por coma


## Definicion
### SPMakeListOfExpressions @str, [@delm], @result = output  
#### @str
*Requerido* <br />
Tipo: `nvarchar(255)`

Cada de caracteres a evaluar

#### @delm
*Opcional* <br />
Tipo: `char(1)` <br />
Por defecto: `','`

Caracter delimitador de cadena de caracteres

#### output
*Requerido* <br />
Tipo: `nvarchar(255) OUTPUT`

Variable que almacena el resultado devuelto de procedimiento almacenado. Parametro `OUTPUT` no esta demas; `OUTPUT` indica que el parametro de entrada es un parametro de salida.

Para mas informacion sobre `OUTPUT` [ver documentacion](https://msdn.microsoft.com/en-us/library/ms187926.aspx).


## Uso
Mejor uso es con sentencias dinamicas.

```sql
declare @whsCodes nvarchar(255)
declare @cmd nvarchar(max)

exec SPMakeListOfExpressions N'02,03,04,05', @result = @whsCodes output

set @cmd =
N'
	SELECT TOP 100 *
	FROM [stocks]
	WHERE ItemCode = N''AIE-010-40X40CM-R''
		AND WhsCode IN (' + @whsCodes + ')'
	
exec sp_executesql @cmd

/** -- Resutado --
ItemCode	WhsCode	OnHand
AIE-010-40X40CM-R	02	0.000000
AIE-010-40X40CM-R	03	0.000000
AIE-010-40X40CM-R	04	0.000000
AIE-010-40X40CM-R	05	0.000000
*/
```