# utl_flag_consecutive_days
Flag Consecutive Days. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Flag Consecutive Days

    github
    https://github.com/rogerjdeangelis/utl_flag_consecutive_days

    Same Result WPS/SAS

    INPUT
    =====


    WORK.HAVE total obs=28

       DATE    ID    NAME

      16840    A      AA
      16841    A      AA
      16842    A      AA
      16843
      16845    A      AA
      16847    A      AA
      16848    A      AA
      16840    B      AA
      16841    B      AA
      16842    B      AA
      16843    B      AA
      16845
      16847    B      AA
      16848    B      AA
      16840
      16841    A      AB
      16842    A      AB
      16843    A      AB
      16845    A      AB
      16847    A      AB
      16848    A      AB
      16840    B      AB
      16841    B      AB
      16842    B      AB
      16843    B      AB
      16845    B      AB
      16847    B      AB


    EXAMPLE OUTPUT


    WORK.WANT total obs=28

      GAP            DATE    FLG    ID    NAME

      NEW SERIES    16840     1     A      AA
      CONTIGUOS     16841     1     A      AA
      CONTIGUOS     16842     1
      CONTIGUOS     16843     1     A      AA

      GAP           16845     0     A      AA

      NEW SERIES    16847     1     A      AA
      CONTIGUOS     16848     1     B      AA

      NEW SERIES    16840     1     B      AA
      CONTIGUOS     16841     1     B      AA
      CONTIGUOS     16842     1     B      AA
      CONTIGUOS     16843     1

      GAP           16845     0     B      AA

      NEW SERIES    16847     1     B      AA


    PROCESS
    =======

    I broke it up into two datastep so you can
    make changes to the second datastep for your rules.

    data havFlg;
      set have;
      difDt=dif(date);
      if difDt=1 then flg=1;
      else flg=0;
    run;quit;

    * correct;
    data want;
      retain gap date flg;
      merge havflg have(firstobs=2 rename=date=nxtDt);
     if  nxtDt-date=1 and flg=0 then flg=1;
     if flg=1 and difDt ne 1 then GAP="NEW SERIES  ";
     else if flg=0 then gap="GAP     ";
     else gap="CONTIGUOS       ";
     drop nxtDt difDt;
    run;quit;

    OUTPUT
    ======

    Up to 40 obs WORK.WANT total obs=28

    Obs    GAP            DATE    FLG    ID    NAME

      1    NEW SERIES    16840     1     A      AA
      2    CONTIGUOS     16841     1     A      AA
      3    CONTIGUOS     16842     1
      4    CONTIGUOS     16843     1     A      AA
      5    GAP           16845     0     A      AA
      6    NEW SERIES    16847     1     A      AA
      7    CONTIGUOS     16848     1     B      AA
      8    NEW SERIES    16840     1     B      AA
      9    CONTIGUOS     16841     1     B      AA
     10    CONTIGUOS     16842     1     B      AA
     11    CONTIGUOS     16843     1
     12    GAP           16845     0     B      AA
     13    NEW SERIES    16847     1     B      AA
     14    CONTIGUOS     16848     1
     15    NEW SERIES    16840     1     A      AB
     16    CONTIGUOS     16841     1     A      AB
     17    CONTIGUOS     16842     1     A      AB
     18    CONTIGUOS     16843     1     A      AB
     19    GAP           16845     0     A      AB
     20    NEW SERIES    16847     1     A      AB
     21    CONTIGUOS     16848     1     B      AB
     22    NEW SERIES    16840     1     B      AB
     23    CONTIGUOS     16841     1     B      AB
     24    CONTIGUOS     16842     1     B      AB
     25    CONTIGUOS     16843     1     B      AB
     26    GAP           16845     0     B      AB
     27    NEW SERIES    16847     1
     28    CONTIGUOS     16848     1


    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;


    data have;
     input date date9. ID $ Name $ ;
    cards4;
    08FEB2006 A AA
    09FEB2006 A AA
    10FEB2006 A AA
    11FEB2006 .  .
    13FEB2006 A AA
    15FEB2006 A AA
    16FEB2006 A AA
    08FEB2006 B AA
    09FEB2006 B AA
    10FEB2006 B AA
    11FEB2006 B AA
    13FEB2006 .  .
    15FEB2006 B AA
    16FEB2006 B AA
    08FEB2006 .  .
    09FEB2006 A AB
    10FEB2006 A AB
    11FEB2006 A AB
    13FEB2006 A AB
    15FEB2006 A AB
    16FEB2006 A AB
    08FEB2006 B AB
    09FEB2006 B AB
    10FEB2006 B AB
    11FEB2006 B AB
    13FEB2006 B AB
    15FEB2006 B AB
    16FEB2006 .  .
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data havFlg;
      set wrk.have;
      difDt=dif(date);
      if difDt=1 then flg=1;
      else flg=0;
    run;quit;

    * correct;
    data wrk.wantwps;
      retain gap date flg;
      merge havflg wrk.have(firstobs=2 rename=date=nxtDt);
     if  nxtDt-date=1 and flg=0 then flg=1;
     if flg=1 and difDt ne 1 then GAP="NEW SERIES  ";
     else if flg=0 then gap="GAP     ";
     else gap="CONTIGUOS       ";
     drop nxtDt difDt;
    run;quit;
    run;quit;
    ');
