# utl-if-a-value-exists-in-a-group-add-new-column-of-ones-else-zeros-sql-sas-r-python-multi-language
If a value exists in a group create new column of 1s else 0 sql sas r python multi language
    %let pgm=utl-if-a-value-exists-in-a-group-add-new-column-of-ones-else-zeros-sql-sas-r-python-multi-language;

    If a value exists in a group create new column of 1s else 0 sql sas r python multi language

    github
    https://tinyurl.com/32veawer
    https://github.com/rogerjdeangelis/utl-if-a-value-exists-in-a-group-add-new-column-of-ones-else-zeros-sql-sas-r-python-multi-language

    Stackoverflow
    https://tinyurl.com/3hdm5u4j
    https://stackoverflow.com/questions/79225564/assign-value-to-all-rows-based-on-condition-being-fulfilled-or-not

      SOLUTIONS

        1 sas sql
        2 r sql
        3 python sql
        4 r dplyr language

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /****************************************************************************************************************************/
    /*                   |                                              |                                                       */
    /*         INPUT     |              PROCESS                         |                OUTPUT                                 */
    /*                   |                                              |                                                       */
    /*                   |                                              |                                                       */
    /*                   |                                              |                                                       */
    /* ID  YEAR  VALUE   | ID  YEAR VALUE  NEWCOLS                      |     ID    YEAR    VALUE    NEWCOL                     */
    /*                   |                                              |                                                       */
    /*  1  2010    1     |  1  2010   1       0                         |      1    2010      1         0                       */
    /*  1  2011    1     |  1  2011   1       0                         |      1    2011      1         0                       */
    /*  1  2012    0     |  1  2012   0       0 has year=2012 & value-0 |      1    2012      0         0 All 0s                */
    /*  1  2013    0     |  1  2013   0       0 so set all newcols to 0 |      1    2013      0         0                       */
    /*  1  2014    0     |  1  2014   0       0                         |      1    2014      0         0                       */
    /*  2  2010    0     |                                              |      2    2010      0         1                       */
    /*  2  2011    1     |                                              |      2    2011      1         1                       */
    /*  2  2012    1     |  2  2010   0       1                         |      2    2012      1         1 All 1s                */
    /*  2  2013    1     |  2  2011   1       1                         |      2    2013      1         1                       */
    /*  2  2014    1     |  2  2012   1       1 not year=2012 & value-0 |      2    2014      1         1                       */
    /*  3  2010    1     |  2  2013   1       1 so set all newcols to 1 |      3    2010      1         0                       */
    /*  3  2011    0     |  2  2014   1       1                         |      3    2011      0         0                       */
    /*  3  2012    0     |                                              |      3    2012      0         0 All 0s                */
    /*  3  2013    0     |  3  2010   1       0                         |      3    2013      0         0                       */
    /*  3  2014    0     |  3  2011   0       0                         |      3    2014      0         0                       */
    /*  4  2010    1     |  3  2012   0       0 has year=2012 & value-0 |      4    2010      1         1                       */
    /*  4  2011    1     |  3  2013   0       0 so set all newcols to 0 |      4    2011      1         1                       */
    /*  4  2012    1     |  3  2014   0       0                         |      4    2012      1         1 All 1s                */
    /*  4  2013    1     |                                              |      4    2013      1         1                       */
    /*  4  2014    1     |  4  2010   1       1                         |      4    2014      1         1                       */
    /*                   |  4  2011   1       1                         |                                                       */
    /*                   |  4  2012   1       1  not year=2012 & value-0|                                                       */
    /*                   |  4  2013   1       1  so set all newcols to 1|                                                       */
    /*                   |                                              |                                                       */
    /*                   |----------------------------------------------|                                                       */
    /*                   |                                              |                                                       */
    /*                   | R SAS PYTHON SAME CODE                       |                                                       */
    /*                   | ======================                       |                                                       */
    /*                   |                                              |                                                       */
    /*                   |  SQL DESCRIBES THE LOGIC?                    |                                                       */
    /*                   |                                              |                                                       */
    /*                   |                                              |                                                       */
    /*                   | Thw inner select provides the the groups     |                                                       */
    /*                   | (group 1 and 3) that satisfy the ops logic   |                                                       */
    /*                   |                                              |                                                       */
    /*                   | OUTPUT OF INNER SELECT                       |                                                       */
    /*                   |                                              |                                                       */
    /*                   |   ID  TRUE                                   |                                                       */
    /*                   |                                              |                                                       */
    /*                   |    1   1   Rows where                        |                                                       */
    /*                   |    2   3   (year=2012)*(value-0))=0          |                                                       */
    /*                   |                                              |                                                       */
    /*                   | The outer select creates  new column of 0s   |                                                       */
    /*                   | when TRUE=1 and new column 1s when false     |                                                       */
    /*                   |                                              |                                                       */
    /*                   | ifn(true,0,1) as newcol                      |                                                       */
    /*                   |                                              |                                                       */
    /*                   |                                              |                                                       */
    /*                   | select                                       |                                                       */
    /*                   |   l.*                                        |                                                       */
    /*                   |  ,ifn(true,0,1) as newcol                    |                                                       */
    /*                   | from                                         |                                                       */
    /*                   |   sd1.have as l left join                    |                                                       */
    /*                   |     (select                                  |                                                       */
    /*                   |        id                                    |                                                       */
    /*                   |       ,1 as true                             |                                                       */
    /*                   |      from                                    |                                                       */
    /*                   |        sd1.have                              |                                                       */
    /*                   |      group                                   |                                                       */
    /*                   |        by id                                 |                                                       */
    /*                   |      having                                  |                                                       */
    /*                   |        max((year=2012)*(value-0))=0) as r    |                                                       */
    /*                   | on                                           |                                                       */
    /*                   |   l.id =r.id                                 |                                                       */
    /*                   | order                                        |                                                       */
    /*                   |   by id, year                                |                                                       */
    /*                   |                                              |                                                       */
    /*                   |----------------------------------------------|                                                       */
    /*                   |                                              |                                                       */
    /*                   | R DPLYR LANGUAGE (%>%,group_by,mutate,any)   |                                                       */
    /*                   | ==========================================   |                                                       */
    /*                   |                                              |                                                       */
    /*                   | want<-have %>%                               |                                                       */
    /*                   |  group_by(ID) %>%                            |                                                       */
    /*                   |  mutate(NEWCOL =                             |                                                       */
    /*                   |                                              |                                                       */
    /*                   |                                              |                                                       */
    /****************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input ID Year Value ;
    cards4;
    1 2010 1
    1 2011 1
    1 2012 0
    1 2013 0
    1 2014 0
    2 2010 0
    2 2011 1
    2 2012 1
    2 2013 1
    2 2014 1
    3 2010 1
    3 2011 0
    3 2012 0
    3 2013 0
    3 2014 0
    4 2010 1
    4 2011 1
    4 2012 1
    4 2013 1
    4 2014 1
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   ID  YEAR  VALUE                                                                                                      */
    /*                                                                                                                        */
    /*    1  2010    1                                                                                                        */
    /*    1  2011    1                                                                                                        */
    /*    1  2012    0                                                                                                        */
    /*    1  2013    0                                                                                                        */
    /*    1  2014    0                                                                                                        */
    /*    2  2010    0                                                                                                        */
    /*    2  2011    1                                                                                                        */
    /*    2  2012    1                                                                                                        */
    /*    2  2013    1                                                                                                        */
    /*    2  2014    1                                                                                                        */
    /*    3  2010    1                                                                                                        */
    /*    3  2011    0                                                                                                        */
    /*    3  2012    0                                                                                                        */
    /*    3  2013    0                                                                                                        */
    /*    3  2014    0                                                                                                        */
    /*    4  2010    1                                                                                                        */
    /*    4  2011    1                                                                                                        */
    /*    4  2012    1                                                                                                        */
    /*    4  2013    1                                                                                                        */
    /*    4  2014    1                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;
    create
      table want as
    select
      l.*
     ,ifn(true,0,1) as newcol
    from
      sd1.have as l left join
        (select
           id
          ,1 as true
         from
           sd1.have
         group
           by id
         having
           max((year=2012)*(value-0))=0) as r
    on
      l.id =r.id
    order
      by id, year
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*    ID    YEAR    VALUE    NEWCOL                                                                                       */
    /*                                                                                                                        */
    /*     1    2010      1         0                                                                                         */
    /*     1    2011      1         0                                                                                         */
    /*     1    2012      0         0                                                                                         */
    /*     1    2013      0         0                                                                                         */
    /*     1    2014      0         0                                                                                         */
    /*     2    2010      0         1                                                                                         */
    /*     2    2011      1         1                                                                                         */
    /*     2    2012      1         1                                                                                         */
    /*     2    2013      1         1                                                                                         */
    /*     2    2014      1         1                                                                                         */
    /*     3    2010      1         0                                                                                         */
    /*     3    2011      0         0                                                                                         */
    /*     3    2012      0         0                                                                                         */
    /*     3    2013      0         0                                                                                         */
    /*     3    2014      0         0                                                                                         */
    /*     4    2010      1         1                                                                                         */
    /*     4    2011      1         1                                                                                         */
    /*     4    2012      1         1                                                                                         */
    /*     4    2013      1         1                                                                                         */
    /*     4    2014      1         1                                                                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    /*--- cannot use column name true )true is the same as constant 1 ---*/

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    library(haven)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    str(have)
    want<-sqldf('
      with
          findids as
      ( select
           id
          ,1 as truth
         from
           have
         group
           by id
         having
           max( (year=2012)*(value-0) ) =0
      )
       select
         l.*
        ,iif(truth,0,1) as newcol
       from
         hav  as l left join findids as r
       on
         l.id =r.id
       order
         by id, year
      ')
    want
    df
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                           |                                                                                            */
    /*  R                        |  SAS                                                                                       */
    /*                           |                                                                                            */
    /*     ID YEAR VALUE newcol  |   ROWNAMES    ID    YEAR    VALUE    NEWCOL                                                */
    /*                           |                                                                                            */
    /*  1   1 2010     1      0  |       1        1    2010      1         0                                                  */
    /*  2   1 2011     1      0  |       2        1    2011      1         0                                                  */
    /*  3   1 2012     0      0  |       3        1    2012      0         0                                                  */
    /*  4   1 2013     0      0  |       4        1    2013      0         0                                                  */
    /*  5   1 2014     0      0  |       5        1    2014      0         0                                                  */
    /*  6   2 2010     0      1  |       6        2    2010      0         1                                                  */
    /*  7   2 2011     1      1  |       7        2    2011      1         1                                                  */
    /*  8   2 2012     1      1  |       8        2    2012      1         1                                                  */
    /*  9   2 2013     1      1  |       9        2    2013      1         1                                                  */
    /*  10  2 2014     1      1  |      10        2    2014      1         1                                                  */
    /*  11  3 2010     1      0  |      11        3    2010      1         0                                                  */
    /*  12  3 2011     0      0  |      12        3    2011      0         0                                                  */
    /*  13  3 2012     0      0  |      13        3    2012      0         0                                                  */
    /*  14  3 2013     0      0  |      14        3    2013      0         0                                                  */
    /*  15  3 2014     0      0  |      15        3    2014      0         0                                                  */
    /*  16  4 2010     1      1  |      16        4    2010      1         1                                                  */
    /*  17  4 2011     1      1  |      17        4    2011      1         1                                                  */
    /*  18  4 2012     1      1  |      18        4    2012      1         1                                                  */
    /*  19  4 2013     1      1  |      19        4    2013      1         1                                                  */
    /*  20  4 2014     1      1  |      20        4    2014      1         1                                                  */
    /*                           |                                                                                            */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    have.info()
    want=pdsql('''
      with
          findids as
      ( select
           id
          ,1 as truth
         from
           have
         group
           by id
         having
           max( (year=2012)*(value-0) ) =0
      )
       select
         l.*
        ,iif(truth,0,1) as newcol
       from
         have as l left join findids as r
       on
         l.id =r.id
       order
         by id, year
       ''');
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;


    /**************************************************************************************************************************/
    /*                                   |                                                                                    */
    /* PYTHON                            |    SAS                                                                             */
    /*                                   |                                                                                    */
    /*       ID    YEAR  VALUE  newcol   |    ID    YEAR    VALUE    NEWCOL                                                   */
    /*                                   |                                                                                    */
    /*  0   1.0  2010.0    1.0       0   |     1    2010      1         0                                                     */
    /*  1   1.0  2011.0    1.0       0   |     1    2011      1         0                                                     */
    /*  2   1.0  2012.0    0.0       0   |     1    2012      0         0                                                     */
    /*  3   1.0  2013.0    0.0       0   |     1    2013      0         0                                                     */
    /*  4   1.0  2014.0    0.0       0   |     1    2014      0         0                                                     */
    /*  5   2.0  2010.0    0.0       1   |     2    2010      0         1                                                     */
    /*  6   2.0  2011.0    1.0       1   |     2    2011      1         1                                                     */
    /*  7   2.0  2012.0    1.0       1   |     2    2012      1         1                                                     */
    /*  8   2.0  2013.0    1.0       1   |     2    2013      1         1                                                     */
    /*  9   2.0  2014.0    1.0       1   |     2    2014      1         1                                                     */
    /*  10  3.0  2010.0    1.0       0   |     3    2010      1         0                                                     */
    /*  11  3.0  2011.0    0.0       0   |     3    2011      0         0                                                     */
    /*  12  3.0  2012.0    0.0       0   |     3    2012      0         0                                                     */
    /*  13  3.0  2013.0    0.0       0   |     3    2013      0         0                                                     */
    /*  14  3.0  2014.0    0.0       0   |     3    2014      0         0                                                     */
    /*  15  4.0  2010.0    1.0       1   |     4    2010      1         1                                                     */
    /*  16  4.0  2011.0    1.0       1   |     4    2011      1         1                                                     */
    /*  17  4.0  2012.0    1.0       1   |     4    2012      1         1                                                     */
    /*  18  4.0  2013.0    1.0       1   |     4    2013      1         1                                                     */
    /*  19  4.0  2014.0    1.0       1   |     4    2014      1         1                                                     */
    /*                                   |                                                                                    */
    /**************************************************************************************************************************/

    /*  _                _       _
    | || |    _ __    __| |_ __ | |_   _ _ __
    | || |_  | `__|  / _` | `_ \| | | | | `__|
    |__   _| | |    | (_| | |_) | | |_| | |
       |_|   |_|     \__,_| .__/|_|\__, |_|
                          |_|      |___/
    */

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    library(haven)
    library(dplyr)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    want<-have %>%
     group_by(ID) %>%
     mutate(NEWCOL =
      ifelse(any(YEAR==2012&VALUE==0),0,1))
    want
    df
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                           |                                                                                            */
    /*  R                        |  SAS                                                                                       */
    /*                           |                                                                                            */
    /*     ID YEAR VALUE newcol  |   ROWNAMES    ID    YEAR    VALUE    NEWCOL                                                */
    /*                           |                                                                                            */
    /*  1   1 2010     1      0  |       1        1    2010      1         0                                                  */
    /*  2   1 2011     1      0  |       2        1    2011      1         0                                                  */
    /*  3   1 2012     0      0  |       3        1    2012      0         0                                                  */
    /*  4   1 2013     0      0  |       4        1    2013      0         0                                                  */
    /*  5   1 2014     0      0  |       5        1    2014      0         0                                                  */
    /*  6   2 2010     0      1  |       6        2    2010      0         1                                                  */
    /*  7   2 2011     1      1  |       7        2    2011      1         1                                                  */
    /*  8   2 2012     1      1  |       8        2    2012      1         1                                                  */
    /*  9   2 2013     1      1  |       9        2    2013      1         1                                                  */
    /*  10  2 2014     1      1  |      10        2    2014      1         1                                                  */
    /*  11  3 2010     1      0  |      11        3    2010      1         0                                                  */
    /*  12  3 2011     0      0  |      12        3    2011      0         0                                                  */
    /*  13  3 2012     0      0  |      13        3    2012      0         0                                                  */
    /*  14  3 2013     0      0  |      14        3    2013      0         0                                                  */
    /*  15  3 2014     0      0  |      15        3    2014      0         0                                                  */
    /*  16  4 2010     1      1  |      16        4    2010      1         1                                                  */
    /*  17  4 2011     1      1  |      17        4    2011      1         1                                                  */
    /*  18  4 2012     1      1  |      18        4    2012      1         1                                                  */
    /*  19  4 2013     1      1  |      19        4    2013      1         1                                                  */
    /*  20  4 2014     1      1  |      20        4    2014      1         1                                                  */
    /*                           |                                                                                            */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
