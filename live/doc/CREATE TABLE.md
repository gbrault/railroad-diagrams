```javascript
/**************************************************************************************************************/ 
 Show(Stack(Title('CREATE TABLE'), 
   Sequence(Terminal('CREATE'),Choice(0,Skip(),Terminal('TEMP'),Terminal('TEMPORARY')),Terminal('TABLE'), 
            Optional(Sequence(Terminal('IF'),Terminal('NOT'),Terminal('EXISTS')) 
   ) /* sequence */ 
 ),Sequence(Optional(Sequence(NonTerminal('schema-name'),Terminal('.'))),NonTerminal('table-name')), 
 /* sequence */ 
 Sequence( 
  Choice(0, 
   Sequence(Terminal('('),OneOrMore(NonTerminal('column-def'),Terminal(',')), 
            ZeroOrMore(Terminal(','),NonTerminal('table-constraint')), 
            Terminal(')'),Optional(Sequence(Terminal('WITHOUT'),Terminal('ROWID')))), 
   Sequence(Terminal('AS'),NonImplemented('select-stm')) 
  ) /* choice */ 
 ), 
 Comment('END CREATE TABLE')) /* Stack */ 
 ); /* Create Table */ 
``` 
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/CREATE TABLE.svg)
``` 
CREATE TABLE: "(`CREATE TABLE` , \n
              ('CREATE' , ('TEMP' | 'TEMPORARY')? , 'TABLE' , ('IF' , 'NOT' , 'EXISTS')?) , \n
              (( schema-name  , '.')? ,  table-name ) , \n
              ((('(' ,  column-def (',' column-def )* , (',' table-constraint )* , ')' , ('WITHOUT' , 'ROWID')?)
              | ('AS' , select-stm))) , \n
              `END CREATE TABLE`)"
```

```javascript
/**************************************************************************************************************/ 
Show( 
 Sequence(Title('schema-name'), 
 NonTerminal('name'),Comment('END schema-name') 
 )  
 ); /* schema name */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/schema-name.svg)
```
schema-name:"(`schema-name` ,  name  , `END schema-name`)"
```

```javascript
/**************************************************************************************************************/ 
Show( 
 Sequence(Title('table-name'),NonTerminal('name'),Comment('END table-name') 
 ) 
 ); /* table name */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/table-name.svg)
```
table-name:"(`table-name` ,  name  , `END table-name`)"
```

```javascript
/**************************************************************************************************************/ 
 Show( 
 Sequence(Title('column-def'),NonTerminal('column-name'),Optional(NonTerminal('type-name')),ZeroOrMore(NonTerminal('column-constraint')), 
 Comment('END column-def') 
 ) 
 ); /* column def */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/column-def.svg)
```
column-def:"(`column-def` ,  column-name  ,  type-name ? , ( column-constraint )* , `END column-def`)"
```

```javascript
/**************************************************************************************************************/
Show( 
 Sequence(Title('column-name'),NonTerminal('name'),Comment('END column-name') 
 ) 
 ); /* column name */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/column-name.svg)
```
column-name:"(`column-name` ,  name  , `END column-name`)"
```

```javascript
/**************************************************************************************************************/ 
 Show( 
 Sequence(Title('name'), 
 Choice(0, 
 	Terminal('/(^[A-Za-z][A-Za-z0-9_]*)/'), 
 	Terminal('/(^\"[A-Za-z][A-Za-z0-9_\s]*)\"/'), 
         Terminal("/(^\'[A-Za-z][A-Za-z0-9_\s]*)\'/") 
 ), 
 Comment('END name') 
 ) 
 ); /* name */
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/name.svg)
```
name:"(`name` , ('/(^[A-Za-z][A-Za-z0-9_]*)/' | '/(^"[A-Za-z][A-Za-z0-9_s]*)"/' | '/(^\'[A-Za-z][A-Za-z0-9_s]*)\'/') , `END name`)"
```

```javascript
/**************************************************************************************************************/
Show(Stack( 
   Title('column-constraint'), 
   Optional(Sequence(Terminal('CONSTRAINT'),NonTerminal('name'))), 
   Sequence(Choice(0, 
          Sequence(Terminal('PRIMARY'),Terminal('KEY'),Choice(0,Skip(),Terminal('ASC'),Terminal('DESC')),NonTerminal('conflict-clause'),Optional(Terminal('AUTOINCREMENT'))), 
          Sequence(Optional(Terminal('NOT')),Terminal('NULL'),NonTerminal('conflict-clause')), 
          Sequence(Terminal('UNIQUE'),NonTerminal('conflict-clause')), 
          Sequence(Terminal('CHECK'),Terminal('('),NonImplemented('expr'),Terminal(')')), 
          Sequence(Terminal('DEFAULT'),Choice(0,NonTerminal('signed-number'),NonTerminal('literal-value'),Sequence(Terminal('('),NonImplemented('expr'),Terminal(')')))), 
          Sequence(Terminal('COLLATE'),Terminal('collation-name')), 
          NonTerminal('foreign-key-clause') 
         )), 
   Comment('END column-constraint') 
   ) 
 ); /* column-constraint */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/column-constraint.svg)
```
column-constraint:"(`column-constraint` , \n('CONSTRAINT' ,  name )? , \n((('PRIMARY' , 'KEY' , ('ASC' | 'DESC')? ,  conflict-clause  , 'AUTOINCREMENT'?)
 | ('NOT'? , 'NULL' ,  conflict-clause ) | ('UNIQUE' ,  conflict-clause ) | ('CHECK' , '(' , expr , ')') | ('DEFAULT' , ( signed-number  |  literal-value  | ('(' , expr , ')')))
 | ('COLLATE' , 'collation-name') |  foreign-key-clause )) , \n`END column-constraint`)"
```

```javascript
/**************************************************************************************************************/
Show(Sequence(Title('conflict-clause'), 
      Optional(Sequence(Terminal('ON'),Terminal('CONFLICT'), 
                                                 Choice(0, 
                                                        Terminal('ROLLBACK'), 
                                                        Terminal('ABORT'), 
                                                        Terminal('FAIL'), 
                                                        Terminal('IGNORE'), 
                                                        Terminal('REPLACE')) 
                                                ) 
                                       ),Comment('END conflict-clause')) 
     ); /* conflict-clause */
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/conflict-clause.svg)
```
conflict-clause:"(`conflict-clause` , ('ON' , 'CONFLICT' , ('ROLLBACK' | 'ABORT' | 'FAIL' | 'IGNORE' | 'REPLACE'))? , `END conflict-clause`)"
```

```javascript
/**************************************************************************************************************/ 
Show(Stack(Title('table-constraint'),Stack(Optional(Sequence(Terminal('CONSTRAINT'),NonTerminal('name'))), 
                                      Choice(0,Sequence(Choice(0,Sequence(Terminal('PRIMARY'),Terminal('Key')),Terminal('UNIQUE')),Terminal('('), 
                                                        OneOrMore(NonTerminal('indexed-column'),Terminal(',')),Terminal(')'),NonTerminal('conflict-clause')), 
                                             Sequence(Terminal('CHECK'),Terminal('('),NonImplemented('expr'),Terminal(')')), 
                                             Sequence(Terminal('FOREIGN'),Terminal('KEY'),Terminal('('), 
                                                      OneOrMore(NonTerminal('column-name'),Terminal(',')),Terminal(')'), 
                                                      NonTerminal('foreign-key-clause')) 
                                            )                                           
      ), 
      Comment('END table-constrainte')) 
 ); /* table-constraint */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/table-constraint.svg)
```
table-constraint:"(`table-constraint` , \n
(('CONSTRAINT' ,  name )? , \n
(((('PRIMARY' , 'Key') | 'UNIQUE') , '(' ,  indexed-column (',' indexed-column )* , ')' ,  conflict-clause )
 | ('CHECK' , '(' , expr , ')') | ('FOREIGN' , 'KEY' , '(' ,  column-name (',' column-name )* , ')' ,  foreign-key-clause )
)) , \n
`END table-constrainte`)"
```

```javascript
/**************************************************************************************************************/
 Show(Stack(Title('indexed-column'), 
      Sequence(Choice(0,NonTerminal('column-name'),NonImplemented('expr')), 
               Optional(Sequence(Terminal('COLLATE'),NonTerminal('collation-name'))), 
               Choice(0,Skip(),Terminal('ASC'),Terminal('DESC'))), 
      Comment('END indexed-column')) 
     ); /* indexed-column */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/indexed-column.svg)
```
indexed-column:"(`indexed-column` , \n
(( column-name  | expr) , ('COLLATE' ,  collation-name )? , ('ASC' | 'DESC')?) , \n
`END indexed-column`)"
```

```javascript
/**************************************************************************************************************/
Show(Stack(Title('type-name'), 
            Sequence(NonTerminal('name'),Choice(0, 
                                                 Skip(), 
                                                 Sequence(Terminal('('),NonTerminal('signed-number'),Terminal(')')), 
                                                 Sequence(Terminal('('),NonTerminal('signed-number'),Terminal(','),NonTerminal('signed-number'),Terminal(')')) 
                                                ) 
                    ), 
            Comment('END type-name')) 
     ); /* type-name */  
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/type-name.svg)
```
type-name:"(`type-name` , \n
( name  , (('(' ,  signed-number  , ')') | ('(' ,  signed-number  , ',' ,  signed-number  , ')')
)?) , \n
`END type-name`)"
```

```javascript
/**************************************************************************************************************/
 Show(Stack(Title('numeric-literal'), 
            Sequence(Choice(0,Choice(0,Sequence(Choice(0,Sequence(Terminal('/[0-9]+/'),Optional(Sequence(Terminal('.'),Terminal('/[0-9]+/')))), 
                                                       Sequence(Terminal('.'),Terminal('/[0-9]+/')) 
                                                      ), 
                                                Optional(Sequence(Terminal('E'), 
                                                                  Choice(0,Skip(),Terminal('+'),Terminal('-')),Terminal('/[0-9]+/'))) 
                                               ) 
                                    ), 
                            Sequence(Terminal('0x'),Terminal('/[A-Za-z0-9]+)/')) 
                           ) 
                    ), 
            Comment('END numeric-literal')) 
     ); /* numeric-literal */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/numeric-literal.svg)
```
numeric-literal:"(`numeric-literal` , \n
(((((('/[0-9]+/' , ('.' , '/[0-9]+/')?) | ('.' , '/[0-9]+/')) , ('E' , ('+' | '-')? , '/[0-9]+/')?)) | 
('0x' , '/[A-Za-z0-9]+)/'))) , \n
 `END numeric-literal`)"
```

```javascript
/**************************************************************************************************************/
Show(Sequence(Title('signed-number'),Optional(Choice(0,Terminal('+'),Terminal('-'))),NonTerminal('numeric-literal'),Comment('END signed-number')) 
     ); /* signed-number */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/signed-number.svg)
```
signed-number:"(`signed-number` , ('+' | '-')? ,  numeric-literal  , `END signed-number`)"
```

```javascript
/**************************************************************************************************************/
Show(Stack(Title('foreign-key-clause'), 
            Sequence(Terminal('REFERENCES'),NonTerminal('foreign-table'), 
                     Optional(Sequence(Terminal('('),OneOrMore(NonTerminal('column-name'),Terminal(',')),Terminal(')')))), 
            Stack(ZeroOrMore( 
                           Choice(0, 
                                  Sequence(Terminal('ON'), 
                                           Choice(0,Terminal('DELETE'),Terminal('UPDATE')), 
                                           Choice(0, 
                                                  Sequence(Terminal('SET'),Choice(0,Terminal('NULL'),Terminal('DEFAULT'))), 
                                                  Terminal('CASCADE'), 
                                                  Terminal('RESTRICT'), 
                                                  Sequence(Terminal('NO'),Terminal('ACTION')) 
                                                 ) 
                                          ), 
                                  Sequence(Terminal('MATCH'),NonTerminal('name')) 
                                 ) 
                           ), 
                           Optional(Sequence(Optional(Terminal('NOT')),Terminal('DEFERRABLE'), 
                                            Choice(0,Skip(),Sequence(Terminal('INITIALLY'), 
                                                                     Choice(0,Terminal('DEFFERED'),Terminal('IMMEDIATE')) 
                                                                    ) 
                                                  ) 
                                            ) 
                                   ) 
                    ), 
            Comment('END foreign-key-clause') 
           ) 
     ); /* foreign-key-clause */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/foreign-key-clause.svg)
```
foreign-key-clause:"(`foreign-key-clause` , \n
('REFERENCES' ,  foreign-table  , ('(' ,  column-name (',' column-name )* , ')')?) , \n
(((('ON' , ('DELETE' | 'UPDATE') , (('SET' , ('NULL' | 'DEFAULT')) | 'CASCADE' | 'RESTRICT' | ('NO' , 'ACTION')))
 | ('MATCH' ,  name )))* , \n
 ('NOT'? , 'DEFERRABLE' , (('INITIALLY' , ('DEFFERED' | 'IMMEDIATE')))?)?) , \n
 `END foreign-key-clause`)"
```

```javascript
/**************************************************************************************************************/
Show(Sequence(Title('foreign-table'),NonTerminal('table-name'),Comment('END foreign-table')) 
     ); /* foreign-table */ 
```
![alt tag](https://gbrault.github.io/railroad-diagrams/live/doc/svg/foreign-table.svg)
```
foreign-table:"(`foreign-table` ,  table-name  , `END foreign-table`)"
```
