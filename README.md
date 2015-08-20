# CoreFMDB

//建立文档
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *documentDirectory = [paths objectAtIndex:0];
NSString *dbPath = [documentDirectory stringByAppendingPathComponent:@"MyDatabase.db"];
FMDatabase *db = [FMDatabase databaseWithPath:dbPath] ;
if (![db open]) {
   NSLog(@“Could not open db.”);
   return ;
   }

//建表

[db executeUpdate:@"CREATE TABLE PersonList (Name text, Age integer, Sex integer, Phone text, Address text, Photo blob)"];

//插入
[db executeUpdate:@"INSERT INTO PersonList (Name, Age, Sex, Phone, Address, Photo) VALUES (?,?,?,?,?,?)",
@"Jone", [NSNumber numberWithInt:20], [NSNumber numberWithInt:0], @“091234567”, @“Taiwan, R.O.C”, [NSData dataWithContentsOfFile: filepath]];

//update 更新
[db executeUpdate:@"UPDATE PersonList SET Age = ? WHERE Name = ?",[NSNumber numberWithInt:30],@“John”];   

//在结果集中查询
FMResultSet *rs = [db executeQuery:@"SELECT Name, Age, FROM PersonList"];
while ([rs next]) {
NSString *name = [rs stringForColumn:@"Name"];
int age = [rs intForColumn:@"Age"];
}
[rs close];

//特征查找

NSString *address = [db stringForQuery:@"SELECT Address FROM PersonList WHERE Name = ?",@"John”];

//找年齡

int age = [db intForQuery:@"SELECT Age FROM PersonList WHERE Name = ?",@"John”];

