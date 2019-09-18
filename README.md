# gold-glove
SQL Server query pulling various defensive metrics used in a model to predict 2019 gold glove winner (excludes pitchers and catchers-only).

select f.Name, f.UZR, w.age, w.fraa, s.Range_and_Positioning,s.GFP_DME, s.Total_Runs_Saved
from fangraphs..fangraphs_defense_2019 f
inner join 
(select Name, max(Inn) Inn
from fangraphs..fangraphs_defense_2019 group by Name) z on f.Name = z.Name and f.Inn = z.Inn
inner join 
(select name, sum(fraa) fraa, max(Age) age
from bp..BP_fielding_2019 group by name) w on f.Name = w.name 
inner join
bis..bis_fielding_2019 s on f.Name = s.Name
order by s.Total_Runs_Saved desc
