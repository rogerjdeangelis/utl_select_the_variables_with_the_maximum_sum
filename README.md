# utl_select_the_variables_with_the_maximum_sum
Select the variables with the maximum sum. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Select the variables with the maximum sum

    This can be applied to other condtions.

    see
    https://github.com/rogerjdeangelis/utl_select_the_variables_with_the_maximum_sum

    inspired by (a different problem)

    https://tinyurl.com/yd47ffzr
    https://stackoverflow.com/questions/50291652/find-the-nth-largest-values-in-the-top-row-and-omit-the-rest-of-the-columns-in-r

    INPUT
    =====

     WORK.HAVE total obs=5

      V1    V2    V3    V4

       1     1     1     1
       2     2     0     2
       3     3     1     0
       4     4     0     1
       5     5     1     2

    EXAMPLE

      V1    V2    V3    V4

      15    15     2     6    ** SUMS

    KEEP JUST VARIABLES V1 and V2


    PROCESS
    =======

    data want;

      * put the variable names with max sum into macro variables;
      if _n_=1 then do;
         %let rc=%sysfunc(dosubl('
            ods output summary=havSum;
            proc means data=sd1.have nway stackodsoutput sum;
            var v:;
            run;quit;
            proc sql;
             select
               variable into :vars separated by " " from havSum having sum=max(sum);
            ;quit;
         '));
       end;

       set have(keep=&vars.);

    run;quit;

    OUTPUT
    ======

      Up to 40 obs WORK.WANT total obs=5

      Obs    V1    V2

       1      1     1
       2      2     2
       3      3     3
       4      4     4
       5      5     5

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
      do row=1 to 5;
         v1=row;
         v2=row;
         v3=mod(row,2);
         v4=mod(row,3);
         output;
      end;
      drop row;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process

