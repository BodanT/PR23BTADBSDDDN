# Umorov in napadov v ZDA
## Člani skupine

| Ime in priimek | Vpisna številka |
| -------------- | --------------- |
| Bodan Temelkovski | 63210463 |
| Anastasija Djajkovska | 63210409 |
| Bojan Spasovski | 63210453 |
| Dimitar Nakov  | 63210442 |
| Danilo Djuric | 63210410 |

## Podatke
Mi bomo uporabljali podatke o umorih v ZDA, populaciji v ZDA in posedovanju orožja v ZDA. Vse te podatki so že podani na “Kaggle” in “World population overview”.
- USA Homicides 1976-2020: https://www.kaggle.com/datasets/stephanieblack1990/usa-homicides-19762020?select=SHR76_20StephanieBlack.xlsx
- Guns per Capita 2023: https://worldpopulationreview.com/state-rankings/guns-per-capita
- 2019 Census US Population Data By State: https://www.kaggle.com/datasets/peretzcohen/2019-census-us-population-data-by-state

## Opis podatkov in atribute

**USA Homicides 1976-2020:**
- Država v ZDA, mesto, datum
- Policijsa agencija, ki je bila odgovorna
- Ali je bil umor razrešen
- Pol, rasa in starost udeležnicev umora
- Način umora, orožje in okoliščine umora
- Sorodnost med morilcem in žrtvijo

**Guns per Capita 2023**
- Država v ZDA
- Podatke o številu prebivalcev te države
- Število registriranih orožij

## Cilj
Naš cilj je natančneje raziskati umore v ZDA, ter raziskati okoliščine in zunanje dejavnike, ki vodijo do umorov in ali obstajajo povezave z načinom njihovega izvajanja.

Vprašanja, ki jih bomo obdelovali:
- Katera zvezna država ZDA ima največ umorov v splošnem?
- Ali odstotek posedovanja orožja posamezne zvezne države vpliva na število umorov v tej zvezni državi?
- Analiza karakteristik morilca in žrtev.
- Katera okolščina največ pripelje do umorov in kakšno srodstvo sta imela morilec in žrtev?
- Analiza razrešitev umora in ali tip agencije vpliva na razrešitvi umora?

###Žačeli bomo z analizo umorov v posamezni zvezni državi
```python
murders_by_state = df_homicides.groupby("State")["ID"].count().reset_index()
murders_by_state.columns = ["State", "Murders"]

state_abbr = {
    'Alabama': 'AL', 'Alaska': 'AK', 'Arizona': 'AZ','Arkansas': 'AR','California': 'CA','Colorado': 'CO','Connecticut': 'CT','Delaware': 'DE','Florida': 'FL','Georgia': 'GA','Hawaii': 'HI','Idaho': 'ID', 'Illinois': 'IL','Indiana': 'IN','Iowa': 'IA','Kansas': 'KS','Kentucky': 'KY','Louisiana': 'LA','Maine': 'ME','Maryland': 'MD','Massachusetts': 'MA','Michigan': 'MI','Minnesota': 'MN','Mississippi': 'MS','Missouri': 'MO','Montana': 'MT','Nebraska': 'NE','Nevada': 'NV','New Hampshire': 'NH','New Jersey': 'NJ','New Mexico': 'NM','New York': 'NY','North Carolina': 'NC','North Dakota': 'ND','Ohio': 'OH','Oklahoma': 'OK','Oregon': 'OR','Pennsylvania': 'PA','Rhodes Island': 'RI','South Carolina': 'SC','South Dakota': 'SD','Tennessee': 'TN','Texas': 'TX','Utah': 'UT','Vermont': 'VT','Virginia': 'VA','Washington': 'WA','West Virginia': 'WV','Wisconsin': 'WI', 'Wyoming': 'WY', 'District of Columbia': 'DC'
}

murders_by_state['State'] = murders_by_state['State'].replace(state_abbr)

fig = px.choropleth(murders_by_state, 
                    locations='State', 
                    locationmode="USA-states", 
                    color='Murders',
                    scope="usa",
                    color_continuous_scale='Blues',
                    range_color=(0, murders_by_state['Murders'].max()),
                    title='Murders per State')
```
![alt text](./map_bez_na_glava_zitel.png)
Iz slike je očitno da največ umorov so v Kaliforniji, vnedar če hočemo biti bolj natančne moramo preračunati število umorov na 100.000 prebivalcev.
```python
murders_by_state = df_homicides.groupby("State")["ID"].count().reset_index()
murders_by_state.columns = ["State", "Murders"]

merged_df = pd.merge(murders_by_state, df_population, on='State')
merged_df['Murder Rate'] = merged_df['Murders'] / merged_df['Population'] * 100000

state_abbr = {
    'Alabama': 'AL', 'Alaska': 'AK', 'Arizona': 'AZ','Arkansas': 'AR','California': 'CA','Colorado': 'CO','Connecticut': 'CT','Delaware': 'DE','Florida': 'FL','Georgia': 'GA','Hawaii': 'HI','Idaho': 'ID', 'Illinois': 'IL','Indiana': 'IN','Iowa': 'IA','Kansas': 'KS','Kentucky': 'KY','Louisiana': 'LA','Maine': 'ME','Maryland': 'MD','Massachusetts': 'MA','Michigan': 'MI','Minnesota': 'MN','Mississippi': 'MS','Missouri': 'MO','Montana': 'MT','Nebraska': 'NE','Nevada': 'NV','New Hampshire': 'NH','New Jersey': 'NJ','New Mexico': 'NM','New York': 'NY','North Carolina': 'NC','North Dakota': 'ND','Ohio': 'OH','Oklahoma': 'OK','Oregon': 'OR','Pennsylvania': 'PA','Rhodes Island': 'RI','South Carolina': 'SC','South Dakota': 'SD','Tennessee': 'TN','Texas': 'TX','Utah': 'UT','Vermont': 'VT','Virginia': 'VA','Washington': 'WA','West Virginia': 'WV','Wisconsin': 'WI', 'Wyoming': 'WY', 'District of Columbia': 'DC'
}

merged_df['State'] = merged_df['State'].replace(state_abbr)

fig = px.choropleth(merged_df, 
                    locations='State', 
                    locationmode="USA-states", 
                    color='Murder Rate',
                    scope="usa",
                    color_continuous_scale='Blues',
                    range_color=(0, merged_df['Murder Rate'].max()),
                    title='Murder Rate per State')
```
![alt text](./map_na_glava_zitel.png.png)
Ker iz slike ne moremo ugotoviti katera država ima največ umorov, smo morali pogledati še en graf in smo ugotovili da ta država je "District of Columbia".
##
