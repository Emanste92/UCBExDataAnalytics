*VBA Assignment*   
Hard + Challenge Difficulty

Screenshots

![ETE_2014_stock](https://github.com/Emanste92/UCBExDataAnalytics/blob/master/HWsubmissions/VBA_Resources/ETE_2014_stock.PNG)
2014 Stock Screenshot

![ETE_2015_stock](https://github.com/Emanste92/UCBExDataAnalytics/blob/master/HWsubmissions/VBA_Resources/ETE_2015_stock.PNG)
2015 Stock Screenshot

![ETE_2016_stock](https://github.com/Emanste92/UCBExDataAnalytics/blob/master/HWsubmissions/VBA_Resources/ETE_2016_stock.PNG)
2016 Stock Screenshot




Stock Data File Completed (DropBox link)
[ETE_Multiple_year_stock_data.xlsm](https://www.dropbox.com/s/etixu426586faun/ETE_Multiple_year_stock_data_All.xlsm?dl=0)


##VBA Script

Sub stock_vols_total():

    ' Defining dimensions of all variables
    Dim ws As Worksheet
    Dim stockname, stockname_next, datestring, Great_Inc_Stock, Great_Dec_Stock, Great_Tot_Stock As String
    Dim i, stock_total, position, LastRow, open_year_value, close_year_value, yearly_change, percent_change, Greatest_Inc, Greatest_Dec, Greatest_Total   As Double
       
    ' Defining that the function applies to all worksheets and all counter variables
    For Each ws In Worksheets
        LastRow = ws.Cells(Rows.Count, 1).End(xlUp).Row
        position = 1
        Greatest_Inc = 0
        Greatest_Dec = 0
        Greatest_Total = 0
        datestring = "firstday"
    
    ' Column headers and formatting
        ws.Range("I1").Value = "Ticker"
        ws.Range("I1").Font.Bold = True
        ws.Range("J1").Value = "Yearly Change"
        ws.Range("J1").Font.Bold = True
        ws.Range("K1").Value = "Percent Change"
        ws.Range("K1").Font.Bold = True
        ws.Range("L1").Value = "Total Stock Volume"
        ws.Range("L1").Font.Bold = True
        ws.Range("O2").Value = "Greatest % Increase"
        ws.Range("O2").Font.Bold = True
        ws.Range("O3").Value = "Greatest % Decrease"
        ws.Range("O3").Font.Bold = True
        ws.Range("O4").Value = "Greatest Total Volume"
        ws.Range("O4").Font.Bold = True
        ws.Range("P1").Value = "Ticker"
        ws.Range("P1").Font.Bold = True
        ws.Range("Q1").Value = "Value"
        ws.Range("Q1").Font.Bold = True
        ws.Range("Q2:Q3").NumberFormat = "0.00%"

    ' Defining initial variables for calculations
            For i = 2 To LastRow
                ws.Cells(i, 11).NumberFormat = "0.00%"

                stockname = ws.Cells(i, 1).Value
                stockname_next = ws.Cells(i + 1, 1).Value
                stock_total = stock_total + ws.Cells(i, 7).Value

    ' Defining the stock opening and closing values and the transition from one stock to the next
                If datestring = "firstday" Then
                    open_year_value = ws.Cells(i, 3).Value
                    datestring = "lastday"
                End If
                
    ' Determining when stockname becomes different and finding the volume sum
                If stockname <> stockname_next Then
                    ws.Cells(position + 1, 9).Value = stockname
                    ws.Cells(position + 1, 12).Value = stock_total
    
    ' Determining the yearly change by subtracting the last <close> value from the
    ' first <open> value for a stock's given year
                    close_year_value = ws.Cells(i, 6).Value
                    yearly_change = close_year_value - open_year_value
                    ws.Cells(position + 1, 10).Value = yearly_change
                    
                        If open_year_value > 0 Then
                            ws.Cells(position + 1, 11).Value = (close_year_value - open_year_value) / open_year_value
       
    ' Determining whether there was a positive or negative change with color conditional formatting
                                If yearly_change > 0 Then
                                ws.Cells(position + 1, 10).Interior.ColorIndex = 4
                                Else
                                ws.Cells(position + 1, 10).Interior.ColorIndex = 3
                                End If
                       
    ' Determining the stock with the greatest increase, stock with the greatest decrease,
    ' and stock with the greatest total volume
                                percent_change = ((close_year_value - open_year_value) / open_year_value)
                                
                                If percent_change > Greatest_Inc Then
                                    Greatest_Inc = percent_change
                                    Greatest_Inc_Stock = stockname
                                End If
                                
                                If percent_change < Greatest_Dec Then
                                    Greatest_Dec = percent_change
                                    Greatest_Dec_Stock = stockname
                                End If
                                
                        End If
                        
                        If stock_total > Greatest_Total Then
                            Greatest_Total = stock_total
                            Greatest_Tot_Stock = stockname
                        End If
    
                    position = position + 1
                    stock_total = 0
                    datestring = "firstday"
                End If
        Next i
        
        ws.Range("P2").Value = Greatest_Inc_Stock
        ws.Range("Q2").Value = Greatest_Inc
        ws.Range("P3").Value = Greatest_Dec_Stock
        ws.Range("Q3").Value = Greatest_Dec
        ws.Range("P4").Value = Greatest_Tot_Stock
        ws.Range("Q4").Value = Greatest_Total
        
    Next ws
    
End Sub
