$result:=""

ARRAY LONGINT($recordsInTable;0)
ARRAY LONGINT($fieldsInTable;0)
ARRAY LONGINT($deletedFieldsInTable;0)
ARRAY LONGINT($fragmentation;0)
ARRAY TEXT($tableName;0)

For ($table;1;Get last table number)
	If (Is table number valid($table))
		$countFields:=Get last field number($table)
		For ($field;1;Get last field number($table))
			If (Not(Is field number valid($table;$field)))
				$countFields:=$countFields-1
			End if 
		End for 
		APPEND TO ARRAY($fieldsInTable;$countFields)
		APPEND TO ARRAY($deletedFieldsInTable;Get last field number($table)-$countFields)
		APPEND TO ARRAY($recordsInTable;Records in table(Table($table)->))
		APPEND TO ARRAY($fragmentation;Get table fragmentation(Table($table)->))
		APPEND TO ARRAY($tableName;Table name($table))
	End if 
End for 

$recordsInDatabase:=Sum($recordsInTable)
For ($i;1;Size of array($tableName))
	$result:=$result+\
	$tableName{$i}+"\t"+\
	String($fragmentation{$i};"##0.00")+"\t"+\
	String($fieldsInTable{$i})+"\t"+\
	String($deletedFieldsInTable{$i})+"\t"+\
	String($recordsInTable{$i}/$recordsInDatabase;"##0.00")+"\r"
End for 

SET TEXT TO PASTEBOARD($result)
