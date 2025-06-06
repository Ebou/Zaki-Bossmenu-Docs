# Zaki Boss Menu - Exports Dokumentation

Denne dokumentation giver en komplet oversigt over alle tilgængelige exports i Zaki Boss Menu systemet. Disse exports gør det muligt for andre scripts at integrere problemfrit med boss menu funktionaliteten.

## Indholdsfortegnelse

1. [Transaktionsstyring](#transaktionsstyring)
2. [Medarbejderstyring](#medarbejderstyring)
3. [Faktureringssystem](#faktureringssystem)
4. [Budgetstyring](#budgetstyring)
5. [Analytics](#analytics)
6. [Indstillingsstyring](#indstillingsstyring)
7. [Discord Integration](#discord-integration)
8. [Zone Styring](#zone-styring)
9. [Medarbejder Metrics](#medarbejder-metrics)
10. [Rapport Generering](#rapport-generering)
11. [Hjælpefunktioner](#hjælpefunktioner)
12. [Brug Eksempler](#brug-eksempler)

---

## Transaktionsstyring

### `addTransaction`
Tilføjer en transaktion til en virksomheds transaktionshistorik.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:addTransaction(society, amount, type, note, source, target, category, description)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn (f.eks. 'police', 'ambulance')
- `amount` (number): Transaktionsbeløb (positivt for indkomst, negativt for udgifter)
- `type` (string): Transaktionstype ('deposit', 'withdraw', 'bonus', osv.)
- `note` (string): Kort transaktionsnotat
- `source` (string, valgfri): Kilde identifier
- `target` (string, valgfri): Mål identifier
- `category` (string, valgfri): Transaktionskategori
- `description` (string, valgfri): Detaljeret beskrivelse

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Tilføj penge fra narkosag til politiet
local success = exports['zaki-bossmenu']:addTransaction('police', 50000, 'deposit', 'Narkosag penge', nil, nil, 'Operationer', 'Penge beslaglagt fra narkosag')
\`\`\`

### `getTransactions`
Henter transaktionshistorik for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getTransactions(society, limit, category)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `limit` (number, valgfri): Maksimalt antal transaktioner (standard: 50)
- `category` (string, valgfri): Filtrer efter kategori

**Returnerer:** `table` - Array af transaktionsobjekter

**Eksempel:**
\`\`\`lua
-- Hent de sidste 20 operationstransaktioner for politiet
local transactions = exports['zaki-bossmenu']:getTransactions('police', 20, 'Operationer')
for _, trans in pairs(transactions) do
    print(trans.note .. ': ' .. trans.amount .. ' kr')
end
\`\`\`

### `getSocietyBalance`
Får den nuværende saldo for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietyBalance(society)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn

**Returnerer:** `number` - Nuværende saldo

**Eksempel:**
\`\`\`lua
-- Check politiets saldo
local balance = exports['zaki-bossmenu']:getSocietyBalance('police')
print('Politiet har ' .. balance .. ' kr på kontoen')
\`\`\`

### `depositToSociety`
Indsætter penge på en virksomhedskonto.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:depositToSociety(society, amount, note, category, description)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `amount` (number): Beløb at indsætte
- `note` (string, valgfri): Transaktionsnotat
- `category` (string, valgfri): Transaktionskategori
- `description` (string, valgfri): Detaljeret beskrivelse

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Indsæt penge fra narkobust
local success = exports['zaki-bossmenu']:depositToSociety('police', 100000, 'Narkobust penge', 'Operationer', 'Penge beslaglagt fra stor narkobust')
if success then
    print('Penge indsat på politiets konto!')
end
\`\`\`

### `withdrawFromSociety`
Hæver penge fra en virksomhedskonto.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:withdrawFromSociety(society, amount, note, category, description)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `amount` (number): Beløb at hæve
- `note` (string, valgfri): Transaktionsnotat
- `category` (string, valgfri): Transaktionskategori
- `description` (string, valgfri): Detaljeret beskrivelse

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Hæv penge til nyt udstyr
local success = exports['zaki-bossmenu']:withdrawFromSociety('police', 25000, 'Nyt udstyr', 'Udstyr', 'Køb af nye taktiske veste')
\`\`\`

---

## Medarbejderstyring

### `getSocietyEmployees`
Får alle medarbejdere i en virksomhed (online og offline).

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietyEmployees(society)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn

**Returnerer:** `table` - Array af medarbejderobjekter

**Eksempel:**
\`\`\`lua
-- Få alle politimedarbejdere
local employees = exports['zaki-bossmenu']:getSocietyEmployees('police')
for _, employee in pairs(employees) do
    local status = employee.online and 'Online' or 'Offline'
    print(employee.name .. ' - ' .. employee.job.grade_label .. ' (' .. status .. ')')
end
\`\`\`

### `hirePlayerToSociety`
Ansætter en spiller i en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:hirePlayerToSociety(playerId, society, grade)
\`\`\`

**Parametre:**
- `playerId` (number): Spiller server ID
- `society` (string): Virksomhedsnavn
- `grade` (number, valgfri): Startgrad (standard: 0)

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Ansæt spiller som politirekrut
local success = exports['zaki-bossmenu']:hirePlayerToSociety(1, 'police', 0)
if success then
    print('Spiller ansat som politirekrut!')
end
\`\`\`

### `firePlayerFromSociety`
Fyrer en spiller fra en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:firePlayerFromSociety(identifier, society)
\`\`\`

**Parametre:**
- `identifier` (string): Spiller identifier
- `society` (string): Virksomhedsnavn

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Fyr en medarbejder
local success = exports['zaki-bossmenu']:firePlayerFromSociety('char1:110000103304f2b', 'police')
\`\`\`

### `promoteEmployee`
Forfremmer en medarbejder til en højere grad.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:promoteEmployee(identifier, society, newGrade)
\`\`\`

**Parametre:**
- `identifier` (string): Spiller identifier
- `society` (string): Virksomhedsnavn
- `newGrade` (number): Ny gradniveau

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Forfrem til sergent (grad 2)
local success = exports['zaki-bossmenu']:promoteEmployee('char1:110000103304f2b', 'police', 2)
\`\`\`

---

## Faktureringssystem

### `createBillForPlayer`
Opretter en regning til en spiller fra en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:createBillForPlayer(society, playerIdentifier, label, amount, dueDate)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `playerIdentifier` (string): Spiller identifier
- `label` (string): Regningsbeskrivelse
- `amount` (number): Regningsbeløb
- `dueDate` (string, valgfri): Forfaldsdato (YYYY-MM-DD format)

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Opret fartbøde
local success = exports['zaki-bossmenu']:createBillForPlayer('police', 'char1:110000103304f2b', 'Fartbøde - 120 km/t i 50 zone', 2500, '2024-01-15')
if success then
    print('Fartbøde oprettet!')
end
\`\`\`

### `getSocietyBills`
Får alle regninger for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietyBills(society, status)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `status` (string, valgfri): Filtrer efter status ('pending', 'paid', 'overdue', 'cancelled')

**Returnerer:** `table` - Array af regningsobjekter

**Eksempel:**
\`\`\`lua
-- Få alle ubetalte regninger
local unpaidBills = exports['zaki-bossmenu']:getSocietyBills('police', 'pending')
print('Antal ubetalte regninger: ' .. #unpaidBills)
\`\`\`

### `payBill`
Markerer en regning som betalt.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:payBill(billId)
\`\`\`

**Parametre:**
- `billId` (number): Regnings ID

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Betal regning automatisk
local success = exports['zaki-bossmenu']:payBill(123)
\`\`\`

### `deleteBill`
Sletter en regning.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:deleteBill(billId)
\`\`\`

**Parametre:**
- `billId` (number): Regnings ID

**Returnerer:** `boolean` - Success status

---

## Budgetstyring

### `createBudget`
Opretter et budget for en virksomhedskategori.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:createBudget(society, category, amount, periodType, startDate, endDate, notes, createdBy)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `category` (string): Budgetkategori
- `amount` (number): Budgetbeløb
- `periodType` (string, valgfri): 'monthly', 'quarterly', 'yearly' (standard: 'monthly')
- `startDate` (string, valgfri): Startdato (YYYY-MM-DD)
- `endDate` (string, valgfri): Slutdato (YYYY-MM-DD)
- `notes` (string, valgfri): Budgetnotater
- `createdBy` (string, valgfri): Opretter identifier

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Opret månedligt udstyrbudget
local success = exports['zaki-bossmenu']:createBudget('police', 'Udstyr', 75000, 'monthly', '2024-01-01', '2024-01-31', 'Månedligt budget til nyt udstyr')
\`\`\`

### `getBudgets`
Får alle budgetter for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getBudgets(society)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn

**Returnerer:** `table` - Array af budgetobjekter med forbrugsinformation

**Eksempel:**
\`\`\`lua
-- Check budgetforbrug
local budgets = exports['zaki-bossmenu']:getBudgets('police')
for _, budget in pairs(budgets) do
    print(budget.category .. ': ' .. budget.spent_amount .. '/' .. budget.budget_amount .. ' kr (' .. budget.percentage_used .. '%)')
end
\`\`\`

---

## Analytics

### `getSocietyAnalytics`
Får analytiske data for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietyAnalytics(society, days)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `days` (number, valgfri): Antal dage at analysere (standard: 30)

**Returnerer:** `table` - Analytics objekt med finansielle data og kategorifordeling

**Eksempel:**
\`\`\`lua
-- Få ugentlige analytics
local analytics = exports['zaki-bossmenu']:getSocietyAnalytics('police', 7)
print('Daglige finansielle data:', json.encode(analytics.daily_financial))
print('Kategorifordeling:', json.encode(analytics.category_breakdown))
\`\`\`

---

## Indstillingsstyring

### `updateSocietySettings`
Opdaterer virksomhedsindstillinger.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:updateSocietySettings(society, customIcon, customWebhook, customColors)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `customIcon` (string, valgfri): Brugerdefineret ikon URL
- `customWebhook` (string, valgfri): Brugerdefineret Discord webhook
- `customColors` (table, valgfri): Brugerdefineret farveskema

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Opdater politiets farver
local colors = {primary = '#1e40af', secondary = '#3b82f6'}
local success = exports['zaki-bossmenu']:updateSocietySettings('police', 'https://example.com/police-icon.png', 'webhook_url', colors)
\`\`\`

### `getSocietySettings`
Får virksomhedsindstillinger.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietySettings(society)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn

**Returnerer:** `table` - Indstillingsobjekt

---

## Discord Integration

### `sendDiscordLog`
Sender en log besked til Discord.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:sendDiscordLog(society, title, description, color, extraFields)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `title` (string): Log titel
- `description` (string): Log beskrivelse
- `color` (number, valgfri): Embed farve (standard: 3066993)
- `extraFields` (table, valgfri): Yderligere embed felter

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Log narkobust til Discord
local fields = {
    {["name"] = "Beløb Beslaglagt", ["value"] = "150.000 kr", ["inline"] = true},
    {["name"] = "Betjent", ["value"] = "John Doe", ["inline"] = true},
    {["name"] = "Lokation", ["value"] = "Grove Street", ["inline"] = true}
}
exports['zaki-bossmenu']:sendDiscordLog('police', 'Narkobust Gennemført', 'Stor narkobust gennemført med succes', 65280, fields)
\`\`\`

---

## Zone Styring

### `getGangZones`
Får zoner kontrolleret af en bande.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getGangZones(gang)
\`\`\`

**Parametre:**
- `gang` (string): Bandenavn

**Returnerer:** `table` - Array af zone objekter med indflydelsesdata

**Eksempel:**
\`\`\`lua
-- Check Vagos territorier
local zones = exports['zaki-bossmenu']:getGangZones('vagos')
for _, zone in pairs(zones) do
    print(zone.zone .. ': ' .. zone.influence .. '% indflydelse')
end
\`\`\`

---

## Medarbejder Metrics

### `addEmployeeMetric`
Tilføjer en præstationsmetrik for en medarbejder.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:addEmployeeMetric(society, employeeIdentifier, metricType, metricValue, periodStart, periodEnd, notes)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `employeeIdentifier` (string): Medarbejder identifier
- `metricType` (string): Metriktype ('arrests', 'sales', 'repairs', osv.)
- `metricValue` (number): Metrikværdi
- `periodStart` (string, valgfri): Periode startdato
- `periodEnd` (string, valgfri): Periode slutdato
- `notes` (string, valgfri): Yderligere notater

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Tilføj arrestationsmetrik
local success = exports['zaki-bossmenu']:addEmployeeMetric('police', 'char1:110000103304f2b', 'arrests', 25, '2024-01-01', '2024-01-31', 'Månedlige arrestationer')
\`\`\`

### `getEmployeeMetrics`
Får medarbejdermetrikker.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getEmployeeMetrics(society, employeeIdentifier, metricType)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `employeeIdentifier` (string, valgfri): Filtrer efter medarbejder
- `metricType` (string, valgfri): Filtrer efter metriktype

**Returnerer:** `table` - Array af metrikobjekter

---

## Rapport Generering

### `generateReport`
Genererer en brugerdefineret rapport.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:generateReport(society, reportName, reportType, reportData, createdBy)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `reportName` (string): Rapportnavn
- `reportType` (string): Rapporttype
- `reportData` (table): Rapportdata
- `createdBy` (string, valgfri): Opretter identifier

**Returnerer:** `number|boolean` - Rapport ID ved succes, false ved fejl

**Eksempel:**
\`\`\`lua
-- Generer månedlig rapport
local reportData = {
    period = '2024-01',
    total_arrests = 150,
    total_fines = 375000,
    drug_busts = 12
}
local reportId = exports['zaki-bossmenu']:generateReport('police', 'Januar Rapport', 'monthly', reportData, 'system')
\`\`\`

### `getReports`
Får rapporter for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getReports(society, reportType, limit)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `reportType` (string, valgfri): Filtrer efter rapporttype
- `limit` (number, valgfri): Maksimalt antal rapporter (standard: 20)

**Returnerer:** `table` - Array af rapportobjekter

---

## Hjælpefunktioner

### `isSocietyBoss`
Tjekker om en spiller er chef for en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:isSocietyBoss(playerId, society)
\`\`\`

**Parametre:**
- `playerId` (number): Spiller server ID
- `society` (string): Virksomhedsnavn

**Returnerer:** `boolean` - True hvis spilleren er chef

**Eksempel:**
\`\`\`lua
-- Check om spiller er politichef
local isBoss = exports['zaki-bossmenu']:isSocietyBoss(1, 'police')
if isBoss then
    print('Spilleren er politichef!')
end
\`\`\`

### `getSocietyInfo`
Får omfattende information om en virksomhed.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:getSocietyInfo(society)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn

**Returnerer:** `table` - Virksomhedsinformationsobjekt

**Eksempel:**
\`\`\`lua
-- Få politiinfo
local info = exports['zaki-bossmenu']:getSocietyInfo('police')
print('Virksomhed: ' .. info.label)
print('Saldo: ' .. info.balance .. ' kr')
print('Medarbejdere: ' .. info.employee_count)
\`\`\`

### `payEmployeeBonus`
Betaler en bonus til en medarbejder.

**Syntax:**
\`\`\`lua
exports['zaki-bossmenu']:payEmployeeBonus(society, employeeId, amount, note)
\`\`\`

**Parametre:**
- `society` (string): Virksomhedsnavn
- `employeeId` (number): Medarbejder server ID
- `amount` (number): Bonusbeløb
- `note` (string, valgfri): Bonusnotat

**Returnerer:** `boolean` - Success status

**Eksempel:**
\`\`\`lua
-- Giv præstationsbonus
local success = exports['zaki-bossmenu']:payEmployeeBonus('police', 1, 10000, 'Fremragende arbejde denne måned')
\`\`\`

---

## Brug Eksempler

### Eksempel 1: Narkobust Integration
\`\`\`lua
-- I dit narkoscript
RegisterServerEvent('drugs:bustComplete')
AddEventHandler('drugs:bustComplete', function(amount, location)
    local source = source
    local xPlayer = ESX.GetPlayerFromId(source)
    
    if xPlayer.job.name == 'police' then
        -- Tilføj penge til politiet
        exports['zaki-bossmenu']:depositToSociety('police', amount, 'Narkobust beslaglæggelse', 'Operationer', 'Penge beslaglagt fra narkobust på ' .. location)
        
        -- Log til Discord
        exports['zaki-bossmenu']:sendDiscordLog('police', 'Narkobust Gennemført', 'Betjent ' .. xPlayer.getName() .. ' gennemførte en narkobust', 65280, {
            {["name"] = "Beløb Beslaglagt", ["value"] = amount .. " kr", ["inline"] = true},
            {["name"] = "Betjent", ["value"] = xPlayer.getName(), ["inline"] = true},
            {["name"] = "Lokation", ["value"] = location, ["inline"] = true}
        })
        
        -- Tilføj præstationsmetrik
        exports['zaki-bossmenu']:addEmployeeMetric('police', xPlayer.identifier, 'drug_busts', 1, os.date('%Y-%m-%d'), os.date('%Y-%m-%d'), 'Narkobust gennemført på ' .. location)
        
        -- Check for månedlig bonus
        local metrics = exports['zaki-bossmenu']:getEmployeeMetrics('police', xPlayer.identifier, 'drug_busts')
        local monthlyBusts = 0
        local currentMonth = os.date('%Y-%m')
        
        for _, metric in pairs(metrics) do
            if string.sub(metric.period_start, 1, 7) == currentMonth then
                monthlyBusts = monthlyBusts + metric.metric_value
            end
        end
        
        -- Bonus for 10+ narkobust på en måned
        if monthlyBusts >= 10 then
            exports['zaki-bossmenu']:payEmployeeBonus('police', source, 25000, 'Månedlig præstationsbonus - 10+ narkobust')
            TriggerClientEvent('esx:showNotification', source, 'Du modtog en præstationsbonus på 25.000 kr!')
        end
    end
end)
\`\`\`

### Eksempel 2: Mekaniker Job Integration
\`\`\`lua
-- I dit mekanikscript
RegisterServerEvent('mechanic:repairComplete')
AddEventHandler('mechanic:repairComplete', function(price, vehicleModel)
    local source = source
    local xPlayer = ESX.GetPlayerFromId(source)
    
    if xPlayer.job.name == 'mechanic' then
        -- Tilføj kommission til mekanikeren
        local commission = math.floor(price * 0.35) -- 35% kommission
        exports['zaki-bossmenu']:depositToSociety('mechanic', commission, 'Reparationskommission', 'Service', 'Kommission fra ' .. vehicleModel .. ' reparation')
        
        -- Tilføj præstationsmetrik
        exports['zaki-bossmenu']:addEmployeeMetric('mechanic', xPlayer.identifier, 'repairs_completed', 1, os.date('%Y-%m-%d'), os.date('%Y-%m-%d'), vehicleModel .. ' reparation gennemført')
        
        -- Check månedlige reparationer for bonus
        local metrics = exports['zaki-bossmenu']:getEmployeeMetrics('mechanic', xPlayer.identifier, 'repairs_completed')
        local monthlyRepairs = 0
        local currentMonth = os.date('%Y-%m')
        
        for _, metric in pairs(metrics) do
            if string.sub(metric.period_start, 1, 7) == currentMonth then
                monthlyRepairs = monthlyRepairs + metric.metric_value
            end
        end
        
        -- Bonus system baseret på antal reparationer
        if monthlyRepairs == 25 then
            exports['zaki-bossmenu']:payEmployeeBonus('mechanic', source, 15000, 'Månedlig milepæl - 25 reparationer')
            TriggerClientEvent('esx:showNotification', source, 'Tillykke! Du nåede 25 reparationer denne måned og modtog en bonus!')
        elseif monthlyRepairs == 50 then
            exports['zaki-bossmenu']:payEmployeeBonus('mechanic', source, 35000, 'Månedlig milepæl - 50 reparationer')
            TriggerClientEvent('esx:showNotification', source, 'Fantastisk! 50 reparationer denne måned - her er din bonus!')
        elseif monthlyRepairs >= 75 then
            exports['zaki-bossmenu']:payEmployeeBonus('mechanic', source, 60000, 'Månedlig milepæl - 75+ reparationer')
            TriggerClientEvent('esx:showNotification', source, 'Utroligt! Du er månedens topmekaniker med 75+ reparationer!')
        end
    end
end)
\`\`\`

### Eksempel 3: Brugerdefineret Bødesystem
\`\`\`lua
-- I dit politiscript
RegisterCommand('givebøde', function(source, args, rawCommand)
    local xPlayer = ESX.GetPlayerFromId(source)
    
    if exports['zaki-bossmenu']:isSocietyBoss(source, 'police') or xPlayer.job.grade >= 2 then
        local targetId = tonumber(args[1])
        local amount = tonumber(args[2])
        local reason = table.concat(args, ' ', 3)
        
        if targetId and amount and reason then
            local xTarget = ESX.GetPlayerFromId(targetId)
            if xTarget then
                -- Opret bøde
                local success = exports['zaki-bossmenu']:createBillForPlayer('police', xTarget.identifier, reason, amount, os.date('%Y-%m-%d', os.time() + 7*24*60*60))
                
                if success then
                    TriggerClientEvent('esx:showNotification', source, 'Bøde oprettet med succes')
                    TriggerClientEvent('esx:showNotification', targetId, 'Du modtog en bøde på ' .. amount .. ' kr for: ' .. reason)
                    
                    -- Log til Discord
                    exports['zaki-bossmenu']:sendDiscordLog('police', 'Bøde Udstedt', 'Betjent udstedte en bøde', 16776960, {
                        {["name"] = "Betjent", ["value"] = xPlayer.getName(), ["inline"] = true},
                        {["name"] = "Modtager", ["value"] = xTarget.getName(), ["inline"] = true},
                        {["name"] = "Beløb", ["value"] = amount .. " kr", ["inline"] = true},
                        {["name"] = "Årsag", ["value"] = reason, ["inline"] = false}
                    })
                    
                    -- Tilføj til betjentens statistik
                    exports['zaki-bossmenu']:addEmployeeMetric('police', xPlayer.identifier, 'fines_issued', 1, os.date('%Y-%m-%d'), os.date('%Y-%m-%d'), 'Bøde udstedt: ' .. reason)
                else
                    TriggerClientEvent('esx:showNotification', source, 'Fejl ved oprettelse af bøde')
                end
            else
                TriggerClientEvent('esx:showNotification', source, 'Spiller ikke fundet')
            end
        else
            TriggerClientEvent('esx:showNotification', source, 'Brug: /givebøde [id] [beløb] [årsag]')
        end
    else
        TriggerClientEvent('esx:showNotification', source, 'Du har ikke tilladelse til at udstede bøder')
    end
end, false)
\`\`\`

### Eksempel 4: Automatisk Budget Overvågning
\`\`\`lua
-- I dit overvågningsscript
CreateThread(function()
    while true do
        Wait(300000) -- Check hver 5. minut
        
        local societies = {'police', 'ambulance', 'mechanic', 'taxi'}
        
        for _, society in pairs(societies) do
            local budgets = exports['zaki-bossmenu']:getBudgets(society)
            
            for _, budget in pairs(budgets) do
                -- Alert hvis budget er 85% brugt
                if budget.percentage_used >= 85 and budget.percentage_used < 95 then
                    exports['zaki-bossmenu']:sendDiscordLog(society, 'Budget Advarsel', 'Budget nærmer sig grænsen', 16776960, {
                        {["name"] = "Kategori", ["value"] = budget.category, ["inline"] = true},
                        {["name"] = "Brugt", ["value"] = budget.percentage_used .. "%", ["inline"] = true},
                        {["name"] = "Tilbage", ["value"] = budget.remaining .. " kr", ["inline"] = true}
                    })
                -- Kritisk alert hvis budget er 95%+ brugt
                elseif budget.percentage_used >= 95 then
                    exports['zaki-bossmenu']:sendDiscordLog(society, 'KRITISK: Budget Næsten Opbrugt', 'Budget er næsten helt opbrugt!', 16711680, {
                        {["name"] = "Kategori", ["value"] = budget.category, ["inline"] = true},
                        {["name"] = "Brugt", ["value"] = budget.percentage_used .. "%", ["inline"] = true},
                        {["name"] = "Tilbage", ["value"] = budget.remaining .. " kr", ["inline"] = true}
                    })
                end
            end
        end
    end
end)
\`\`\`

### Eksempel 5: Ambulance Integration
\`\`\`lua
-- I dit ambulancescript
RegisterServerEvent('ambulance:playerRevived')
AddEventHandler('ambulance:playerRevived', function(targetId)
    local source = source
    local xPlayer = ESX.GetPlayerFromId(source)
    local xTarget = ESX.GetPlayerFromId(targetId)
    
    if xPlayer.job.name == 'ambulance' then
        -- Tilføj penge for behandling
        local treatmentFee = 2500
        exports['zaki-bossmenu']:depositToSociety('ambulance', treatmentFee, 'Patientbehandling', 'Medicinske Tjenester', 'Behandling af ' .. xTarget.getName())
        
        -- Tilføj præstationsmetrik
        exports['zaki-bossmenu']:addEmployeeMetric('ambulance', xPlayer.identifier, 'patients_treated', 1, os.date('%Y-%m-%d'), os.date('%Y-%m-%d'), 'Patient genoplivet')
        
        -- Log til Discord
        exports['zaki-bossmenu']:sendDiscordLog('ambulance', 'Patient Behandlet', 'Paramediciner behandlede en patient', 65280, {
            {["name"] = "Paramediciner", ["value"] = xPlayer.getName(), ["inline"] = true},
            {["name"] = "Patient", ["value"] = xTarget.getName(), ["inline"] = true},
            {["name"] = "Behandlingsgebyr", ["value"] = treatmentFee .. " kr", ["inline"] = true}
        })
        
        -- Check for månedlig bonus baseret på behandlinger
        local metrics = exports['zaki-bossmenu']:getEmployeeMetrics('ambulance', xPlayer.identifier, 'patients_treated')
        local monthlyTreatments = 0
        local currentMonth = os.date('%Y-%m')
        
        for _, metric in pairs(metrics) do
            if string.sub(metric.period_start, 1, 7) == currentMonth then
                monthlyTreatments = monthlyTreatments + metric.metric_value
            end
        end
        
        -- Bonus for mange behandlinger
        if monthlyTreatments == 30 then
            exports['zaki-bossmenu']:payEmployeeBonus('ambulance', source, 20000, 'Månedlig præstationsbonus - 30 behandlinger')
            TriggerClientEvent('esx:showNotification', source, 'Du modtog en bonus for 30 behandlinger denne måned!')
        end
    end
end)
\`\`\`

---

## Fejlhåndtering

Alle exports inkluderer grundlæggende fejlhåndtering og vil returnere `false` eller tomme tabeller ved fejl. Tjek altid returværdier:

\`\`\`lua
local success = exports['zaki-bossmenu']:addTransaction('police', 5000, 'deposit', 'Test')
if success then
    print('Transaktion tilføjet med succes')
else
    print('Fejl ved tilføjelse af transaktion')
end
\`\`\`

## Performance Overvejelser

- Brug passende grænser når du henter store datasæt
- Cache ofte tilgået data når det er muligt
- Overvej at bruge async mønstre til databaseoperationer
- Overvåg Discord webhook rate limits

## Almindelige Use Cases

### 1. **Narkobust Scripts**
- Automatisk indsættelse af beslaglagte penge
- Præstationssporing for betjente
- Discord logging af operationer

### 2. **Job Scripts (Mekaniker, Taxi, osv.)**
- Kommissionssystem
- Præstationsbaserede bonusser
- Automatisk budgetsporing

### 3. **Bødesystemer**
- Automatisk regningsoprettelse
- Betjentstatistikker
- Discord logging

### 4. **Gang/Mafia Scripts**
- Territoriestyring
- Indtægtssporing
- Konfliktlogging

### 5. **Event Scripts**
- Præmieuddelinger
- Turneringsgebyrer
- Sponsorindtægter

## Support

**Discord:** [[Discord Server](https://discord.gg/5CRTp6sz62)]
**GitHub:** [[GitHub Repository](https://github.com/Ebou/Zaki-Bossmenu-Docs/edit/main/EXPORTS_DOKUMENTATION.md)]


