# jdbf
A java reader writer for DBF database file: jdbf

Source forked from: https://code.google.com/archive/p/jdbf/

## Changes:
### Modified by: Aeric Poon
### Modified date: 27 Dec 2021
### Description:
* Added some missing field types (G, T)
* Added parse for timestamp type
* Added check for more logical values
* Do not trim String with spaces

# Introduction
A common java reader writer library for the DBF database file.
The original http://code.google.com/p/java-dbf/ will be removed. 
It's very easy to use.


# Example
## Write a DBF:
```
    public static void main(String args[]) throws Exception
    {
        JDBField[] fields = { new JDBField("ID", 'C', 8, 0), new JDBField("Name", 'C', 254, 0),
                new JDBField("TestN", 'N', 20, 0), // 第三个参数值一定不大于20
                new JDBField("TestF", 'F', 20, 6), // F类型与N类型同,且第四个参数值有小数位数，否则会截短
                new JDBField("TestD", 'D', 8, 0) };
        DBFWriter dbfwriter = new DBFWriter("./testwrite.dbf", fields);

        Object[][] records = { { "1", "hexiong ", new Integer(500), new Double(500.123), new Date() },
                { "2", " hefang ", new Integer(600), new Double(600.234), new Date() },
                { "3", "hexi01234567890123456789012345678901234567890123456789"
                       +"0123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789"
                       +"0123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789", 
                    new Integer(600), new Double(600.234), new Date() },
                { "4", "heqiang", new Integer(700), new Double(700.456), new Date() } };

        for (int i = 0; i < records.length; i++)
        {
            dbfwriter.addRecord(records[i]);
        }
        dbfwriter.close();
        System.out.println("testwrite.dbf write finished.......");

        testRead();
    }
```

## Read a dbf: 
```
    public static void main(String args[]) throws Exception
    {
        //DBFReader dbfreader = new DBFReader((new URL("http://www.svcon.com/us48st.dbf")).openStream());
        //DBFReader dbfreader = new DBFReader("F:\\work\\book2.dbf");
        DBFReader dbfreader = new DBFReader("./book2.dbf");
        int i;
        for (i=0; i<dbfreader.getFieldCount(); i++) {
          System.out.print(dbfreader.getField(i).getName()+"  ");
        }
        System.out.print("\n");
        for(i = 0; dbfreader.hasNextRecord(); i++)
        {
            Object aobj[] = dbfreader.nextRecord(Charset.forName("GBK"));
            for (int j=0; j<aobj.length; j++)
              System.out.print(aobj[j]+"  |  ");
            System.out.print("\n");
        }

        System.out.println("Total Count: " + i);
    }
```
