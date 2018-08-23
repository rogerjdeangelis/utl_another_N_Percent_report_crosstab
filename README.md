# utl_another_N_Percent_report_crosstab
Another N Percent report crosstab.  Another N Percent report crosstab
    Another N Percent report crosstab

    github
    https://tinyurl.com/y897ovhb
    https://github.com/rogerjdeangelis/utl_another_N_Percent_report_crosstab

    SAS Forum
    https://tinyurl.com/ybng73ye
    https://communities.sas.com/t5/ODS-and-Base-Reporting/Proc-Tabulate-N/m-p/489385

    see other reporting tips
    https://tinyurl.com/ycnbdamo
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=report&type=&language=

    INPUT
    =====

     WORK.HAVE total obs=19

       SEX    AGECAT

        M     AGE15
        F     AGE12
        F     AGE12
        F     AGE15
       ....
        M     AGE15
        M     AGE12
        M     AGE15


    EXAMPLE OUTPUT
    ==============

     WORK.WANT total obs=3

       LABEL     AGE12YR       AGE15YR       AGETOT

        F       5(50.0%)      4(44.4%)     9(47.4%)
        M       5(50.0%)      5(55.6%)     10(52.6%)

        Sum     10(100.0%)    9(100.0%)    19(100.0%)


    PROCESS
    =======

    ods exclude all;
    ods output observed =xpocnt;
    proc corresp data=have all dimens=1 print=both;
     tables   sex,agecat;
    run;quit;
    ods select all;

    data WANT;
        set xpocnt(where=(label='Sum'));
        age12Sum=age12;
        age15Sum=age15;
        ageSum  =Sum;
        do until (dne);
          set xpocnt end=dne;
          age12yr    = cats(put(age12,4.),'(',put(100*age12/age12Sum,5.1),'%)');
          age15yr    = cats(put(age15,4.),'(',put(100*age15/age15Sum,5.1),'%)');
          ageTot    = cats(put(Sum,4.),'(',put(100*sum/ageSum  ,5.1),'%)');
          output;
        end;
        keep label age12yr age15yr agetot;
    run;quit;


    OUTPUT
    ======

     WORK.WANT total obs=3

       LABEL     AGE12YR       AGE15YR       AGETOT

        F       5(50.0%)      4(44.4%)     9(47.4%)
        M       5(50.0%)      5(55.6%)     10(52.6%)

        Sum     10(100.0%)    9(100.0%)    19(100.0%)

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
      set sashelp.class(keep=sex age);
      agecat='AGE'!!put(round(age,3),2.);
      drop age;
    run;quit;

