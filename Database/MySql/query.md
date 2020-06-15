select measure_timestamp,assetId,measurepoint,value from
    (
    select measure_timestamp from
        (
        select m_t,m_p,P_N,exp,lead(exp,1,null) as expr,
                               lead(exp,2,null) as expr1,
                               lead(exp,3,null) as expr2,
                               lead(exp,1,null) as expr3,
                              over(partionby m_t, orderby P_N) from
            (
            select m_t, case when m_p = V_LOT_NO then 1
                            when m_p = ${P1} then 2
                            when m_p = ${P2} then 3
                            when m_p = ${P3} then 4
                            else 0
                        end as P_N,
                        
                        case when m_p = V_LOT_NO and (value = ${value1} or value = ${value2}) then true
                            when m_p = ${P1} and (value >= ${valStart} and value <= ${valEnd}) then true
                            when m_p = ${P2} and (value >= ${valStart} and value <= ${valEnd}) then true
                            when m_p = ${P3} and (value >= ${valStart} and value <= ${valEnd}) then true
                            else false
                        end as exp,

                        from asset_measure_value
                        when m_t between ${timeStart} and ${timeEnd}
                        and asset_id = ${assetId}  
                        and P_N !=0
            )         
        ) where expr = true  and expr1 = true or expr2 =true and expr3 = true      
    ) 