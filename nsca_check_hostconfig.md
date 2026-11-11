====================================================        

### WARNING: branding logos not installed ###

RUN:   

`/usr/local/scripts/infra/fix_branding.sh -f`

====================================================        

### WARNING: Cluster Size patch not applied ###
RUN:     

`/usr/local/scripts/infra/apply_pvepatch.sh cluster_size`

====================================================        
          

### WARNNG: query-blockstats enabled ###

RUN:   

`/usr/local/scripts/infra/disable_blockstats.sh apply`

====================================================        

