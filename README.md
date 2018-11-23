# utl-skip-the-next-datastep-if-an-error-occurs
Skip the next datastep if an error occurs.
    Skip the next datastep if an error occurs

    github
    https://tinyurl.com/y9ydom9d
    https://github.com/rogerjdeangelis/utl-skip-the-next-datastep-if-an-error-occurs

    SAS Forum
    https://tinyurl.com/y86ockz3
    https://communities.sas.com/t5/SAS-Programming/How-to-stop-sas-from-processing-next-data-step/m-p/515419


    If there is a an error in the first datastep (creates chek.sas.7bat)
    the second datastep will not be executed.

    A detailed log and a log dataset is also created along wit the log.



    INPUT
    =====

      Datastep with an error

         * sashelp.classem does not exist;

         data check;
            set sashelp.classem;
         run;quit;


    EAMPLE OUTPUT
    -------------

    LOG
    ----

    Failed to Create Dataset Check
    Error Code =1012
    Error Text =File SASHELP.CLASSEM.DATA does not exist.


    LOG Dataset
    -----------

    WORK.LOG total obs=1

      SYSERR                  SYSERRORTEXT

       1012     File SASHELP.CLASSEM.DATA does not exist.

    WORK.WANT was not created


    PROCESS
    =======

    data log(drop rc);

      if _n_=0 then do; %let rc=%sysfunc(dosubl('
         data check;
            * clossem does not exist;
            set sashelp.classem;
         run;quit;
         %let rc_code=&syserr;
         %let rctxt=&syserrortext;
         '));
      end;

      if symgetn('rc_code') ne 0 then do;

             syserr=symget('rc_code');
             syserrortext=symget('rctxt');

             putlog // "Failed to Create Dataset Check" //;

             putlog "Error Code =" syserr //;
             putlog "Error Text =" syserrortext //;
             output;
             stop;

      end;
      else do;
        rc=dosubl('
           data want;
             set sashelp.class;
           run;quit;
        ');
      end;

    run;quit;

    OUTPUT
    ======

     see above

    MAKE DATA
    =========

    data internal


