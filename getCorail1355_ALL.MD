# for 1355 we will not have "raw" files - everything will be stored in this markdown file

> README for patch 003 will be just a copy - nothing really special

## getCorail1355_000

```M
let
    var1partflowliststr = "Z-PurchaseOrderList.xlsx",
    var2 = 
        Replacer.ReplaceText(getCurrentPath(), "DECORATOR", "CORAIL_SRC"),
    pathForCorailSrc = var2,
    out01 = 
        Folder.Files(pathForCorailSrc),
    #"Filtered Rows" = 
        Table.SelectRows(out01, each Text.EndsWith([Name], var1partflowliststr)),
    #"attr_hidden" = 
        Table.SelectRows(#"Filtered Rows", each [Attributes]?[Hidden]? <> true),
    #"transformedColumn" = 
        Table.AddColumn(#"attr_hidden", "TABLES", each transform([Content])),
    #"Expanded TABLES" = 
        Table.ExpandTableColumn(transformedColumn, "TABLES", 
            {"Product", "Product description", "Purchase order number", "Status", "PF status", 
            "Cancelled", "Seller", "Company name", "City", "Postal code", "Country", "Shipper", 
            "Company name_1", "City_2", "Postal code_3", "Country_4", "Manufacturer", "Company name_5", 
            "City_6", "Postal code_7", "Country_8", "User Center", "Purchasing division", "Rank 2", "DTS", 
            "DFS", "Incoterms", "DA Quota", "Type", "Label type", "Label code nature", "GAP DA", "GA", "GA Label", 
            "With belonging?", "CORAIL Creation", "XF SELLER", "XF SHIPPER", "XF MANUFACTURER"}, 
            {"Product", "Product description", "Purchase order number", "Status", "PF status", 
            "Cancelled", "Seller", "Company name", "City", "Postal code", "Country", "Shipper", 
            "Company name_1", "City_2", "Postal code_3", "Country_4", "Manufacturer", "Company name_5", 
            "City_6", "Postal code_7", "Country_8", "User Center", "Purchasing division", "Rank 2", "DTS", 
            "DFS", "Incoterms", "DA Quota", "Type", "Label type", "Label code nature", "GAP DA", "GA", "GA Label", 
            "With belonging?", "CORAIL Creation", "XF SELLER", "XF SHIPPER", "XF MANUFACTURER"}),
    #"Duplicated Column" = 
        Table.DuplicateColumn(#"Expanded TABLES", "Name", "Name - Copy"),
    #"Split Column by Delimiter" = 
        Table.SplitColumn(#"Duplicated Column", "Name - Copy", 
        Splitter.SplitTextByEachDelimiter({"Z-"}, QuoteStyle.Csv, false), 
        {"Name - Copy.1", "Name - Copy.2"}),
    #"Changed Type" = 
        Table.TransformColumnTypes(#"Split Column by Delimiter", 
        {{"Name - Copy.1", type text}, {"Name - Copy.2", type text}}),
    #"Renamed Columns" = 
        Table.RenameColumns(#"Changed Type",{{"Name - Copy.1", "rawTime"}}),
    #"Split Column by Delimiter1" = 
        Table.SplitColumn(#"Renamed Columns", "rawTime", 
        Splitter.SplitTextByEachDelimiter({"T"}, QuoteStyle.Csv, false), {"rawTime.1", "rawTime.2"}),
    #"Changed Type1" = 
        Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"rawTime.1", type date}, {"rawTime.2", type text}}),
    #"Removed Columns" = 
        Table.RemoveColumns(#"Changed Type1",{"rawTime.2"})
in
    #"Removed Columns"
```



## getCorail1355_001:

```M
let
    Source = getCorail1355_000,
    #"Sorted Rows" = 
        Table.Sort(Source,{{"rawTime.1", Order.Descending}}),
    #"Removed Columns" = 
        Table.RemoveColumns(#"Sorted Rows",{"Name - Copy.2", "Content", "Extension", 
        "Date accessed", "Date modified", "Date created", "Attributes"}),
    #"Added Index" = 
        Table.AddIndexColumn(#"Removed Columns", "Index", 0, 1, Int64.Type),
    #"Removed Duplicates" = 
        Table.Distinct(#"Added Index", {"Product", "Seller", "Shipper", "Manufacturer", "User Center"})
in
    #"Removed Duplicates"
```




## getCOrail1355_002:


```M
let
    Source = getCorail1355_001,
    #"Removed Columns" = 
        Table.RemoveColumns(Source,{"Name", "Folder Path"}),
    #"Renamed Columns" = 
        Table.RenameColumns(#"Removed Columns",{{"Product", "PN"}, 
        {"Product description", "PN name"}, 
        {"Purchase order number", "PO Number"}}),
    #"Removed Columns1" = 
        Table.RemoveColumns(#"Renamed Columns",{"Status"}),
    #"Renamed Columns1" = 
        Table.RenameColumns(#"Removed Columns1",{{"Seller", "Seller COFOR"}, 
        {"Company name", "Seller Name"}, {"City", "Seller City"}, 
        {"Postal code", "Seller Postal code"}, {"Country", "Seller Country"}, 
        {"Shipper", "Shipper COFOR"}, {"Company name_1", "Shipper Name"}, 
        {"City_2", "Shipper City"}, {"Postal code_3", "Shipper Postal code"}, 
        {"Country_4", "Shipper Country"}, {"Manufacturer", "Manufacturer COFOR"}, 
        {"Company name_5", "Manufacturer Name"}, {"City_6", "Manufacturer City"}, 
        {"Postal code_7", "Manufacturer Postal code"}, 
        {"Country_8", "Manufacturer Country"}}),
    #"Removed Columns2" = 
        Table.RemoveColumns(#"Renamed Columns1",{"Rank 2", "DA Quota", "rawTime.1", "Index"})
in
    #"Removed Columns2"
```


### OEF
