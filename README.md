# utl-creating-a-crosstab-output-sas-dataset-with-just-one-proc
Creating a crosstab output sas dataset with just one proc 
    Creating a crosstab output sas dataset with just one proc                                                                           
                                                                                                                                        
    OP wants a sasdataset thta looks like the protabulate static report                                                                 
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/ybf9qtjj                                                                                                        
    https://github.com/rogerjdeangelis/utl-creating-a-crosstab-output-sas-dataset-with-just-one-proc                                    
                                                                                                                                        
    SAS forum                                                                                                                           
    https://tinyurl.com/y9ofewoa                                                                                                        
    https://communities.sas.com/t5/SAS-Procedures/How-can-i-return-the-output-datasets-from-Proc-Tabulate/m-p/667113                    
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/ruak6u3                                                                                                         
    https://github.com/rogerjdeangelis/utl-conformal-ods-table-output-for-proc-tabulate-and-proc-freq                                   
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/y7dxgedy                                                                                                        
    https://github.com/rogerjdeangelis/utl-crosstab-output-tables-from-corresp-report-not-static-tabulate                               
                                                                                                                                        
    related repo                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/w7qxsyx                                                                                                         
    https://github.com/rogerjdeangelis/utl-fixing-ods-output-bug-in-proc-freq-crosstab-creating-ods-crosstab-table                      
                                                                                                                                        
    other repos                                                                                                                         
    https://github.com/rogerjdeangelis?tab=repositories&q=utl_odsrpt+in%3Areadme&type=&language=                                        
                                                                                                                                        
    macros                                                                                                                              
    https://tinyurl.com/y9nfugth                                                                                                        
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                          
                                                                                                                                        
    /*               _     _                                                                                                            
     _ __  _ __ ___ | |__ | | ___ _ __ ___                                                                                              
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \                                                                                             
    | |_) | | | (_) | |_) | |  __/ | | | | |                                                                                            
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|                                                                                            
    |_|                                                                                                                                 
    */                                                                                                                                  
                                                                                                                                        
    PROBLEM (Tabulate does not honor ODS)                                                                                               
                                                                                                                                        
       Four Solutions                                                                                                                   
         a. proc correcp                                                                                                                
         b. proc tabulate                                                                                                               
         c, proc report                                                                                                                 
         d. proc freq                                                                                                                   
                                                                                                                                        
                                                                                                                                        
    Problem the ods or output dataset from proc report does not honor ods.                                                              
    The output dataset is not want the op wanted.                                                                                       
    He wanted a rectagular table like the one below.                                                                                    
                                                                                                                                        
    proc tabulate data=sashelp.class out=temp;                                                                                          
    class age sex;                                                                                                                      
    table age='',sex=''*N='';                                                                                                           
    run;                                                                                                                                
                                                                                                                                        
    --------------------------------------------------------------------                                                                
    |                                        |     F      |     M      |                                                                
    |----------------------------------------+------------+------------|                                                                
    |11                                      |        1.00|        1.00|                                                                
    |----------------------------------------+------------+------------|                                                                
    |12                                      |        2.00|        3.00|                                                                
    |----------------------------------------+------------+------------|                                                                
    |13                                      |        2.00|        1.00|                                                                
    |----------------------------------------+------------+------------|                                                                
    |14                                      |        2.00|        2.00|                                                                
    |----------------------------------------+------------+------------|                                                                
    |15                                      |        2.00|        2.00|                                                                
    |----------------------------------------+------------+------------|                                                                
    |16                                      |           .|        1.00|                                                                
    --------------------------------------------------------------------                                                                
                                                                                                                                        
     Up to 40 obs from TEMP total obs=11                                                                                                
                                                                                                                                        
     Obs    AGE    SEX    _TYPE_    _PAGE_    _TABLE_    N                                                                              
                                                                                                                                        
       1     11     F       11         1         1       1                                                                              
       2     11     M       11         1         1       1                                                                              
       3     12     F       11         1         1       2                                                                              
       4     12     M       11         1         1       3                                                                              
       5     13     F       11         1         1       2                                                                              
       6     13     M       11         1         1       1                                                                              
       7     14     F       11         1         1       2                                                                              
       8     14     M       11         1         1       2                                                                              
       9     15     F       11         1         1       2                                                                              
      10     15     M       11         1         1       2                                                                              
      11     16     M       11         1         1       1                                                                              
                                                                                                                                        
    /*                                                                                                                                  
      ___ ___  _ __ _ __ ___  ___ _ __                                                                                                  
     / __/ _ \| `__| `__/ _ \/ __| `_ \                                                                                                 
    | (_| (_) | |  | | |  __/\__ \ |_) |                                                                                                
     \___\___/|_|  |_|  \___||___/ .__/                                                                                                 
                                 |_|                                                                                                    
    */                                                                                                                                  
    ods exclude all;                                                                                                                    
    ods output observed=wantcor;                                                                                                        
    proc corresp data=sashelp.class dim=1 observed;                                                                                     
    table age, sex;                                                                                                                     
    run;quit;                                                                                                                           
    ods select all;                                                                                                                     
                                                                                                                                        
    ods select none;                                                                                                                    
    ods output observed=want_cor;                                                                                                       
    proc corresp data=have dim=1 observed;                                                                                              
    tables &vars1,&vars2;                                                                                                               
    run;quit;                                                                                                                           
    ods select all;                                                                                                                     
                                                                                                                                        
    /*        _           _       _                                                                                                     
    | |_ __ _| |__  _   _| | __ _| |_ ___                                                                                               
    | __/ _` | `_ \| | | | |/ _` | __/ _ \                                                                                              
    | || (_| | |_) | |_| | | (_| | ||  __/                                                                                              
     \__\__,_|_.__/ \__,_|_|\__,_|\__\___|                                                                                              
                                                                                                                                        
    */                                                                                                                                  
                                                                                                                                        
                                                                                                                                        
    title;                                                                                                                              
    %utl_odstab(setup);                                                                                                                 
    proc tabulate data=sashelp.class;                                                                                                   
    class age sex;                                                                                                                      
    table age='',sex=''*N=''/box="Age";                                                                                                 
    run;quit;                                                                                                                           
    %utl_odstab(outdsn=want);                                                                                                           
                                                                                                                                        
    proc print data=want(drop=var:);                                                                                                    
    run;quit;                                                                                                                           
                                                                                                                                        
      J1AGE             J2F             J3M                                                                                             
                                                                                                                                        
         11               1               1                                                                                             
         12               2               3                                                                                             
         13               2               1                                                                                             
         14               2               2                                                                                             
         15               2               2                                                                                             
         16               .               1                                                                                             
    /*                         _                                                                                                        
     _ __ ___ _ __   ___  _ __| |_                                                                                                      
    | `__/ _ \ `_ \ / _ \| `__| __|                                                                                                     
    | | |  __/ |_) | (_) | |  | |_                                                                                                      
    |_|  \___| .__/ \___/|_|   \__|                                                                                                     
             |_|                                                                                                                        
    */                                                                                                                                  
                                                                                                                                        
    %utl_odsrpt(setup);                                                                                                                 
    proc report data=sashelp.class nowd missing noheader box formchar='|'; ;                                                            
    title "|age|F|M|";                                                                                                                  
    cols age sex;                                                                                                                       
    define age / group;                                                                                                                 
    define sex / across;                                                                                                                
    run;quit;                                                                                                                           
    %utl_odsrpt(outdsn=wantrpt);                                                                                                        
                                                                                                                                        
    Up to 40 obs from WANTRPT total obs=6                                                                                               
                                                                                                                                        
    Obs    AGE    F    M                                                                                                                
                                                                                                                                        
     1      11    1    1                                                                                                                
     2      12    2    3                                                                                                                
     3      13    2    1                                                                                                                
     4      14    2    2                                                                                                                
     5      15    2    2                                                                                                                
     6      16    .    1                                                                                                                
                                                                                                                                        
    /*__                                                                                                                                
     / _|_ __ ___  __ _                                                                                                                 
    | |_| `__/ _ \/ _` |                                                                                                                
    |  _| | |  __/ (_| |                                                                                                                
    |_| |_|  \___|\__, |                                                                                                                
                     |_|                                                                                                                
    */                                                                                                                                  
    ods ls=255 ps=200;                                                                                                                  
    %utl_odsfrq(setup);                                                                                                                 
    proc freq data =sashelp.class;                                                                                                      
    tables age*sex;                                                                                                                     
    run;quit;                                                                                                                           
    %utl_odsfrq(outdsn=wantfrq);                                                                                                        
                                                                                                                                        
    proc print data=wantfrq(where=(rownam="COUNT"));                                                                                    
    run;quit;                                                                                                                           
                                                                                                                                        
    Obs    ROWNAM    LEVEL               F               M           TOTAL                                                              
                                                                                                                                        
      1    COUNT      11                 1               1               2                                                              
      5    COUNT      12                 2               3               5                                                              
      9    COUNT      13                 2               1               3                                                              
     13    COUNT      14                 2               2               4                                                              
     17    COUNT      15                 2               2               4                                                              
     21    COUNT      16                 0               1               1                                                              
                                                                                                                                        
                                                                                                                                        
                                                                                                                                        
                                                                                                                                        
