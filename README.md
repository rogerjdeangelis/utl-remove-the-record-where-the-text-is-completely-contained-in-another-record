# utl-remove-the-record-where-the-text-is-completely-contained-in-another-record
Remove the record where the text is completely contained in another record 
    Remove the record where the text is completely contained in another record                                                 
                                                                                                                               
    github                                                                                                                     
    https://tinyurl.com/vyrxhes                                                                                                
    https://github.com/rogerjdeangelis/utl-remove-the-record-where-the-text-is-completely-contained-in-another-record          
                                                                                                                               
    SAS Forum                                                                                                                  
    https://communities.sas.com/t5/SAS-Programming/Remove-similarities/m-p/606884                                              
                                                                                                                               
    Profile Ksharp (nice solution mabe not the fastest but data can be partitioned ie mod(id,16)=0-15                          
    https://communities.sas.com/t5/user/viewprofilepage/user-id/18408                                                          
                                                                                                                               
    *_                   _                                                                                                     
    (_)_ __  _ __  _   _| |_                                                                                                   
    | | '_ \| '_ \| | | | __|                                                                                                  
    | | | | | |_) | |_| | |_                                                                                                   
    |_|_| |_| .__/ \__,_|\__|                                                                                                  
            |_|                                                                                                                
    ;                                                                                                                          
    data have;                                                                                                                 
      input ID Transport $ 3-103;                                                                                              
      infile datalines truncover;                                                                                              
    cards4;                                                                                                                    
    1 Singapore Car A                                                                                                          
    1 Car A                                                                                                                    
    1 UK                                                                                                                       
    1 UK Van CA                                                                                                                
    2 Airplane GA                                                                                                              
    2 Japan Airplane GA                                                                                                        
    ;;;;                                                                                                                       
    run;quit;                                                                                                                  
                                                                                                                               
                              | RULES                                                                                          
     WORK.HAVE total obs=6    |                                                                                                
                              |                                                                                                
      ID    TRANSPORT         |                                                                                                
                              |                                                                                                
       1    Singapore Car A   |                                                                                                
       1    Car A             | Remove: because 'CAR A' is in 'Singapore Car A'                                                
       1    UK                | Remove: because 'UK' is in 'UK Van CA'                                                         
       1    UK Van CA         |                                                                                                
                                                                                                                               
       2    Airplane GA       | Remove because 'Airplane GA' it is in 'Japan Airplane GA'                                      
       2    Japan Airplane GA |                                                                                                
                                                                                                                               
    *            _               _                                                                                             
      ___  _   _| |_ _ __  _   _| |_                                                                                           
     / _ \| | | | __| '_ \| | | | __|                                                                                          
    | (_) | |_| | |_| |_) | |_| | |_                                                                                           
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                          
                    |_|                                                                                                        
    ;                                                                                                                          
                                                                                                                               
     WORK.WANT total obs=3                                                                                                     
                                                                                                                               
      ID    TRANSPORT                                                                                                          
                                                                                                                               
       1    Singapore Car A                                                                                                    
       1    UK Van CA                                                                                                          
       2    Japan Airplane GA                                                                                                  
                                                                                                                               
    *                                                                                                                          
     _ __  _ __ ___   ___ ___  ___ ___                                                                                         
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                        
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                        
    | .__/|_|  \___/ \___\___||___/___/                                                                                        
    |_|                                                                                                                        
    ;                                                                                                                          
                                                                                                                               
    proc sql;                                                                                                                  
                                                                                                                               
      create                                                                                                                   
          table want as                                                                                                        
      select                                                                                                                   
          distinct a.*                                                                                                         
      from                                                                                                                     
          have as a,have as b                                                                                                  
      where                                                                                                                    
          a.id=b.id and                                                                                                        
          a.Transport ne b.Transport  and                                                                                      
          a.Transport contains strip(b.Transport)                                                                              
    ;                                                                                                                          
    quit;                                                                                                                      
                                                                                                                               
