Dates = 
VAR  BaseCalendar = CALENDAR(DATE(2014,1,1),TODAY())

RETURN
GENERATE(
        BaseCalendar, 
        VAR BaseDate = [Date]
        VAR YearDate = YEAR(BaseDate)
        VAR MonthNumber = MONTH( BaseDate)
        VAR FiscalYear = IF(MonthNumber<=6,CONCATENATE(YearDate-1,CONCATENATE("-",YearDate)),CONCATENATE(YearDate,CONCATENATE("-",YearDate+1)))
        VAR CurrentFY = IF(MONTH(TODAY())<=6,CONCATENATE(YEAR(TODAY())-1,CONCATENATE("-",YEAR(TODAY()))),CONCATENATE(YEAR(TODAY()),CONCATENATE("-",YEAR(TODAY())+1)))
        VAR FilterFY = IF(FiscalYear=CurrentFY,"Yes","No")
        VAR OrderMonth = IF(MonthNumber<=6,MonthNumber+12,MonthNumber)
        VAR CurrentMonth = IF(MONTH(TODAY())<=6,MONTH(TODAY())+12,MONTH(TODAY()))
        VAR FilterMonth = IF(IF(MonthNumber<=6,MonthNumber+12,MonthNumber)=CurrentMonth,"Yes","No")
        VAR FilterPreviousMonth = IF(IF(MonthNumber<=6,MonthNumber+12,MonthNumber)=CurrentMonth-1,"Yes","No")
        VAR QuarterFY = IF(MonthNumber<=3,"Q3",IF(MonthNumber<=6,"Q4",IF(MonthNumber<=9,"Q1","Q2")))
        VAR YTDMONTH = IF(OrderMonth<=CurrentMonth-1,"YTD","")
        VAR Week = WEEKNUM(BaseDate,1)
        VAR Biweek = IF(ISODD(Week),Week,Week-1)
        VAR WeekOneDate =  [Date] - WEEKDAY([Date],1) + 1
        VAR Minweek =  IF(ISODD(Week), [Date] - WEEKDAY([Date],1) + 1,[Date] - WEEKDAY([Date],1) -6)
        RETURN ROW( "Day", BaseDate,
                    "Year", YearDate,
                    "Month Number", MonthNumber,
                    "Month", FORMAT(BaseDate,"mmmm"),
                    "Year Month", FORMAT(BaseDate,"mmm yy"),
                    "Fiscal Year",FiscalYear,
                    "Order", OrderMonth,
                    "YTD", YTDMONTH,
                    "Quarter FY", QuarterFY,
                    "Week", Week,
                    "BIWeek", Biweek,
                    "SingleWeekDate",WeekOneDate,
                    "WeekDate", Minweek,
                    "Current FY", FilterFY,
                    "Current Month", FilterMonth,
                    "Previous Month", FilterPreviousMonth
        )
)
