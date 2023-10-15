##getCorail1320_000


```M
let
    var1partflowliststr = "Z-PartFlowList.xlsx",
    var2 = Replacer.ReplaceText(getCurrentPath(), "DECORATOR", "CORAIL_SRC"),
    pathForCorailSrc = var2,
    out01 = Folder.Files(pathForCorailSrc),
    #"Filtered Rows" = Table.SelectRows(out01, each Text.EndsWith([Name], var1partflowliststr)),
    #"attr_hidden" = Table.SelectRows(#"Filtered Rows", each [Attributes]?[Hidden]? <> true),
    transformedColumn = Table.AddColumn(#"attr_hidden", "TABLES", each transform([Content])),
    #"Expanded TABLES" = Table.ExpandTableColumn(transformedColumn, "TABLES", {"Produit", "Désignation", "Vendeur", "Raison Sociale", "Expéditeur", "Raison Sociale_1", "Fabricant", "Société du fabricant", "Centre Utilisateur", "DDU", "DTS", "DFS", "% Actif", "St FP", "ID", "Division", "Nom site", "Statut FP", "Périmètre appro", "Id Appro", "Type de flux", "Mode de calcul", "Type de module", "Quais", "Famille", "Systèmes d'ordre", "NEXT", "ZIF EXCEPTION", "Composant", "Flux particulier", "UC nominal reçue", "UA/UM opérationnelle validée", "Flux de transport créé", "Lead time appro lointain", "Système d'ordre", "Pourcentage de répartition logistique validé", "Commande reçue", "Incoterm transportation condition", "Flux de communication pour envoi de prévisions", "Flux de communication pour envoi d'ordres", "Quai", "CAT", "Sécu transport", "Sécu douane", "Validation manuelle"}, {"Produit", "Désignation", "Vendeur", "Raison Sociale", "Expéditeur", "Raison Sociale_1", "Fabricant", "Société du fabricant", "Centre Utilisateur", "DDU", "DTS", "DFS", "% Actif", "St FP", "ID", "Division", "Nom site", "Statut FP", "Périmètre appro", "Id Appro", "Type de flux", "Mode de calcul", "Type de module", "Quais", "Famille", "Systèmes d'ordre", "NEXT", "ZIF EXCEPTION", "Composant", "Flux particulier", "UC nominal reçue", "UA/UM opérationnelle validée", "Flux de transport créé", "Lead time appro lointain", "Système d'ordre", "Pourcentage de répartition logistique validé", "Commande reçue", "Incoterm transportation condition", "Flux de communication pour envoi de prévisions", "Flux de communication pour envoi d'ordres", "Quai", "CAT", "Sécu transport", "Sécu douane", "Validation manuelle"}),
    #"Duplicated Column" = Table.DuplicateColumn(#"Expanded TABLES", "Name", "Name - Copy"),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Duplicated Column", "Name - Copy", Splitter.SplitTextByEachDelimiter({"Z-"}, QuoteStyle.Csv, false), {"Name - Copy.1", "Name - Copy.2"}),
    #"Changed Type" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Name - Copy.1", type text}, {"Name - Copy.2", type text}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Name - Copy.1", "rawTime"}}),
    #"Split Column by Delimiter1" = Table.SplitColumn(#"Renamed Columns", "rawTime", Splitter.SplitTextByEachDelimiter({"T"}, QuoteStyle.Csv, false), {"rawTime.1", "rawTime.2"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"rawTime.1", type date}, {"rawTime.2", type text}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type1",{"rawTime.2"})
in
    #"Removed Columns"
```
