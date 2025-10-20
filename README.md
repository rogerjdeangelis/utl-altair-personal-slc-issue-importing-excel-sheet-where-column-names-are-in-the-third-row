# utl-altair-personal-slc-issue-importing-excel-sheet-where-column-names-are-in-the-third-row
Altair personal slc issue importing excel sheet where column names are in the third row
    %let pgm=utl-altair-personal-slc-issue-importing-excel-sheet-where-column-names-are-in-the-third-row;

    %stop_submission;

    Altair personal slc issue importing excel sheet where column names are in the third row

    Too long to post on a listserf see
    github
    https://github.com/rogerjdeangelis/utl-altair-personal-slc-issue-importing-excel-sheet-where-column-names-are-in-the-third-row

      CONTENTS

         1 libname excel fails.   Drops first 3 rows of data.
         2 proc import datarow=3? Ignores Datarow?
         3 r openxlsx

    SOAPBOX ON
      I am new to the altair slc so I may not have the corect combination
      of options.
    SOAPBOX OFF


    /********************************************************************************************************/
    /*      INPUT d:/xls/startrow.xlsx       | LIBNAME IMPORTED   | PROC IMPORT            | R IMPORTED     */
    /*                                       | WPD DATASET        | WPD DATASET            | DATASET(CORECT)*/
    /* -------------------------+            |                    |                        |                *
    /* | A1| fx DATE: 10/19/2025|            |   NAME   SEX  AGE  | DATE                   |                */
    /* ------------------------------------  |                    | __10_19_2025 VAR2 VAR3 |  NAME  SEX AGE */
    /* [_] |    A     |    B    |    C    |  |  Barbara  F    13  |                        |                */
    /* ------------------------------------  |  Carol    F    14  |   NAME       SEX  AGE  | Alfred  M   14 */
    /*  1  | DATE: 10/19/2025             |  |  Henry    M    14  |   Alfred     M    14   | Alice   F   13 */
    /*  -- | TIME: 12:15PM                +  |  James    M    12  |   Alice      F    13   | Barbara F   13 */
    /*  2  |                              |  |                    |   Barbara    F    13   | Carol   F   14 */
    /*  -- |----------+---------+---------+  |                    |   Carol      F    14   | Henry   M   14 */
    /*  3  | NAME     |   SEX   |   AGE   |  |                    |   Henry      M    14   | James   M   12 */
    /*  -- |----------+---------+---------+  |                    |   James      M    12   |                */
    /*  4  |   Alfred |  M      |  14     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /*  5  |   Alice  |  F      |  13     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /*  6  |   Barbara|  F      |  13     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /*  7  |   Carol  |  F      |  14     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /*  8  |   Henry  |  M      |  14     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /*  9  |   James  |  M      |  12     |  |                    |                        |                */
    /*  -- |----------+---------+---------+  |                    |                        |                */
    /* [have]                                |                    |                        |                */
    /********************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data have;
      input
        name$
        sex$
        age;
    cards4;
    Alfred  M 14
    Alice   F 13
    Barbara F 13
    Carol   F 14
    Henry   M 14
    James   M 12
    ;;;;
    run;quit;

    %utlfkil(d:/xls/startrow.xlsx);

    ods excel file="d:/xls/startrow.xlsx" options(sheet_name="have" embedded_titles="YES");

    proc print data=have noobs;
    title1 "DATE: 10/19/2025";
    title2 "TIME: 12:15PM";
    run;quit;

    ods excel close;

    /*   _ _ _                                                    _   __       _ _
    / | | (_) |__  _ __   __ _ _ __ ___   ___    _____  _____ ___| | / _| __ _(_) |___
    | | | | | `_ \| `_ \ / _` | `_ ` _ \ / _ \  / _ \ \/ / __/ _ \ || |_ / _` | | / __|
    | | | | | |_) | | | | (_| | | | | | |  __/ |  __/>  < (_|  __/ ||  _| (_| | | \__ \
    |_| |_|_|_.__/|_| |_|\__,_|_| |_| |_|\___|  \___/_/\_\___\___|_||_|  \__,_|_|_|___/
    */


    EXCEL ENGINE FAILS
    ===================

    proc datasets lib=work nodetails nolist;
      delete datarow3;
    run;quit;

    libname xls excel "d:/xls/startrow.xlsx" ;

    data datarow3;
      set have(firstobs=3);
    run;quit;

    libname xls clear;

    proc print data=datarow3;
    run;quit;

    XLSX ENGINE FAILS (SAME RESULT)
    ===============================

    proc datasets lib=work nodetails nolist;
      delete datarow3;
    run;quit;

    libname xls xlsx "d:/xls/startrow.xlsx" ;

    data datarow3;
      set have(firstobs=3);
    run;quit;

    libname xls clear;

    proc print data=datarow3;
    run;quit;

                                     ============================
    OUTPUT  COLUMN NAMES CORRECT BUT MISSING FIRST 2 ROWS OF DATA
    ==============================================================

    Obs     NAME      SEX    AGE

     1     Barbara     F      13
     2     Carol       F      14
     3     Henry       M      14
     4     James       M      12

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    1916
    1917
    1918      proc datasets lib=work nodetails nolist;
    1919        delete datarow3;
    1920      run;quit;
    NOTE: Deleting "WORK.DATAROW3" (memtype="DATA")
    NOTE: Procedure datasets step took :
          real time       : 0.001
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:43:20
          Peak working set    : 91176k
          Current working set : 82768k
          Page fault count    : 0


    1921
    1922      libname xls xlsx "d:/xls/startrow.xlsx";
    NOTE: Library xls assigned as follows:
          Engine:        XLSX
          Physical Name: d:\xls\startrow.xlsx

    1923
    1924      data datarow3;
    1925        set have(firstobs=3);
    1926      run;

    NOTE: 4 observations were read from "WORK.have"
    NOTE: Data set "WORK.datarow3" has 4 observation(s) and 3 variable(s)
    NOTE: The data step took :
          real time       : 0.002
          user cpu time   : 0.000
          system cpu time : 0.015
          Timestamp       :   19OCT25:14:43:20
          Peak working set    : 91176k
          Current working set : 82780k
          Page fault count    : 18


    1926    !     quit;
    NOTE: Libref XLS has been deassigned.
    1927
    1928      libname xls clear;
    1929
    1930      proc print data=datarow3;
    1931      run;quit;
    NOTE: 4 observations were read from "WORK.datarow3"
    NOTE: Procedure print step took :
          real time       : 0.016
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:43:20
          Peak working set    : 91176k
          Current working set : 82780k
          Page fault count    : 4


    /*___                                                      _
    |___ \   _ __  _ __ ___   ___    _____  ___ __   ___  _ __| |_
      __) | | `_ \| `__/ _ \ / __|  / _ \ \/ / `_ \ / _ \| `__| __|
     / __/  | |_) | | | (_) | (__  |  __/>  <| |_) | (_) | |  | |_
    |_____| | .__/|_|  \___/ \___|  \___/_/\_\ .__/ \___/|_|   \__|
            |_|                              |_|
    */

    proc datasets lib=work nodetails nolist;
      delete want;
    run;quit;

    proc import datafile="d:/xls/startrow.xlsx"
                dbms=xlsx
                out=want
                replace;
                getname=yes;
                datarow=3;
                sheet=have;
    run;

    proc print data=want;
    run;


    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    OUTPUT (IGNORES DATAROW)
    ========================

    DATE: 10/19/2025
    TIME: 12:15PM

           DATE
    Obs    __10_19_2025    VAR2    VAR3

     1       NAME          SEX     AGE
     2       Alfred        M       14
     3       Alice         F       13
     4       Barbara       F       13
     5       Carol         F       14
     6       Henry         M       14
     7       James         M       12



    1844      proc datasets lib=work nodetails nolist;
    1845        delete want;
    1846      run;quit;
    NOTE: WORK.WANT (memtype="DATA") was not found, and has not been deleted
    NOTE: Procedure datasets step took :
          real time       : 0.000
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:35:49
          Peak working set    : 91176k
          Current working set : 83020k
          Page fault count    : 0


    1847
    1848      proc import datafile="d:/xls/startrow.xlsx"
    1849                  dbms=xlsx
    1850                  out=want
    1851                  replace;
    1852
    1853                  datarow=3;
    1854                  sheet=have;
    1855      run;
    NOTE: Procedure import step took :
          real time       : 0.000
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:35:49
          Peak working set    : 91176k
          Current working set : 83020k
          Page fault count    : 0


    1856      libname _XLSXIMP xlsx "d:\xls\startrow.xlsx" access=readonly
    1857      header=YES
    NOTE: Library _XLSXIMP assigned as follows:
          Engine:        XLSX
          Physical Name: d:\xls\startrow.xlsx

    1858      datarow=3
    1859      ;
    1860      data want;
    1861      set _XLSXIMP.'have'n;
    1862      ;
    1863      run;

    NOTE: 7 observations were read from "_XLSXIMP.have"
    NOTE: Data set "WORK.want" has 7 observation(s) and 3 variable(s)
    NOTE: The data step took :
          real time       : 0.003
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:35:49
          Peak working set    : 91176k
          Current working set : 83020k
          Page fault count    : 16


    NOTE: Libref _XLSXIMP has been deassigned.
    1864      libname _XLSXIMP clear;
    1865
    1866      proc print data=want;
    1867      run;
    NOTE: 7 observations were read from "WORK.want"
    NOTE: Procedure print step took :
          real time       : 0.015
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   19OCT25:14:35:49
          Peak working set    : 91176k
          Current working set : 83020k
          Page fault count    : 2

    /*____                                      _
    |___ /   _ __    ___  _ __   ___ _ __ __  _| |_____  __
      |_ \  | `__|  / _ \| `_ \ / _ \ `_ \\ \/ / / __\ \/ /
     ___) | | |    | (_) | |_) |  __/ | | |>  <| \__ \>  <
    |____/  |_|     \___/| .__/ \___|_| |_/_/\_\_|___/_/\_\
                         |_|
    */

    &_init_;
    proc r;
    submit;
    library(openxlsx)
    df <- as.data.frame(read.xlsx("d:/xls/startrow.xlsx", startRow = 3, colNames = TRUE))
    head(df)
    endsubmit;
    import data=df r=df;
    run;quit;

    proc print;
    run;

    OUTPUT
    ======

    Obs     NAME      SEX    AGE

     1     Alfred      M      14
     2     Alice       F      13
     3     Barbara     F      13
     4     Carol       F      14
     5     Henry       M      14
     6     James       M      12

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    SYMBOLGEN: Macro variable _init_ resolved to      ods _all_ close;   ods listing;   options ls=255 ps=65     nofmterr nocenter     nodate nonumber     noquotelenmax     validvarname=upcase     compress=no     FORMCHAR='|----|+|---+=|-/\<>*'
    SYMBOLGEN: Some characters in the above value which were subject to macro quoting have been unquoted for printing
    2087
    2088      &_init_;
    2089      proc r;
    2090      submit;
    2091      library(openxlsx)
    2092      df <- as.data.frame(read.xlsx("d:/xls/startrow.xlsx", startRow = 3, colNames = TRUE))
    2093      head(df)
    2094      endsubmit;
    NOTE: Using R version 4.5.1 (2025-06-13 ucrt) from d:\r451

    NOTE: Submitting statements to R:

    > library(openxlsx)
    > df <- as.data.frame(read.xlsx("d:/xls/startrow.xlsx", startRow = 3, colNames = TRUE))

    NOTE: Processing of R statements complete

    > head(df)
    2095      import data=df r=df;
    NOTE: Creating data set 'WORK.df' from R data frame 'df'
    NOTE: Data set "WORK.df" has 6 observation(s) and 3 variable(s)

    2096      run;quit;
    NOTE: Procedure r step took :
          real time       : 2.480
          user cpu time   : 0.000
          system cpu time : 0.031
          Timestamp       :   20OCT25:06:28:39
          Peak working set    : 91176k
          Current working set : 45940k
          Page fault count    : 173


    2097
    2098      proc print;
    2099      run;
    NOTE: 6 observations were read from "WORK.df"
    NOTE: Procedure print step took :
          real time       : 0.010
          user cpu time   : 0.000
          system cpu time : 0.000
          Timestamp       :   20OCT25:06:28:39
          Peak working set    : 91176k
          Current working set : 45940k
          Page fault count    : 4


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */


