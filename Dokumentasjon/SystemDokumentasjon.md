<summary>
  <h2>Krav</h2>
</summary>
  
  - Klient-side for brukergrensesnitt med støtte for at brukere kan registrere treningsøkter med dato, type trening og varighet
  - Visning av treningshistorikk med grafer over utviklingen. Data relatert til treningsøkter skal være registrert per. bruker
  - Brukerspesifikk data må lagres i en database på en server.
  - Tilgjengelig og brukervennlig design for alle brukergrupper.

</details>

<details open>
<summary>
  <h2>Teknologier</h2>
</summary>
  
  - Vue.js
  - Omega 365 CTP (Appframe)
  - Bootstrap
  - Github
  - DrawSQL
  - Figma
  - Highcharts
  
</details>

<details open>
<summary>
  <h2>Arkitektur</h2>
</summary>
<ul>
    <li>
      <details>
        <summary>
          <h4>Tabeller</h4>:
        </summary>      
          <img src="https://github.com/user-attachments/assets/e8360b96-7831-4723-ba27-ed61a8449f47"/>
      </details>
    </li>
    <li>
      <details>
        <summary>
          <h4>Views</h4> 
        </summary>
        <table>
          <tr>
            <th>Navn</th>
            <th>Beskrivelse</th>
            <th>Kode</th>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_Exercises</b></td>
            <td>
              Dette viewet viser exercises, med type .
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT
                    E.ID, E.PrimKey, E.Created, E.CreatedBy_ID, E.Updated, E.UpdatedBy_ID,
                    E.Name, E.IsAssisted, E.IsStock, ET.Name AS TypeName
                        FROM dbo.atbv_WorkOut_Exercises AS E
                            LEFT OUTER JOIN dbo.atbl_WorkOut_Exercise_Types AS ET
                                ON ET.ID = E.[Type_ID]
              </code></pre>
              </details>
            </td>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_Sets</b></td>
            <td>
              Dette viewet viser sets, og sier om exercisen er assisted.
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT
                    S.ID, S.PrimKey, S.Created, S.CreatedBy_ID, S.Updated, S.UpdatedBy_ID,
                    S.Session_ID, S.Weight, S.Repetitions, S.Exercise_ID, E.IsAssisted
                        FROM dbo.atbv_WorkOut_Sets AS S
                            INNER JOIN dbo.atbl_WorkOut_Exercises AS E
                                ON E.ID = S.Exercise_ID
              </code></pre>
              </details>
            </td>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_Exercise_Routines</b></td>
            <td>
              Dette viewet viser many to many tabellen WorkOut_Exercie_Routines, og har exersice name, routine name og is assisted.
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT
                    ER.ID, ER.PrimKey, ER.Created, ER.CreatedBy_ID, ER.Updated, ER.UpdatedBy_ID,
                    ER.DefaultSetAmount, ER.Routine_ID, ER.Exercise_ID, ER.[Order], E.Name AS ExerciseName,
                    R.Name AS RoutineName, E.IsAssisted
                        FROM dbo.atbv_WorkOut_Exercise_Routines AS ER
                            INNER JOIN dbo.atbv_WorkOut_Exercises AS E
                                ON E.ID = ER.Exercise_ID
                            INNER JOIN dbo.atbv_WorkOut_Routines AS R
                                ON R.ID = ER.Routine_ID
              </code></pre>
              </details>
            </td>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_TotalAmountLifted</b></td>
            <td>
              Dette viewet viser hvor mye vekt man har løftet til sammen.
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT CONCAT(ISNULL(CAST(SUM(S.Weight * S.Repetitions) AS INT), 0), ' kg') AS TotalAmount
                      FROM dbo.atbv_WorkOut_Sets AS S
              </code></pre>
              </details>
            </td>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_Session_Sets</b></td>
            <td>
              Dette viewet viser hvor mange sets det er per exercise og hvilken session de tilhører.
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT
                    SS.ID AS Session_ID, E.Name AS ExerciseName, COUNT(S.ID) AS SetCount
                        FROM dbo.atbv_WorkOut_Session AS SS
                            INNER JOIN dbo.atbv_WorkOut_Sets AS S
                                ON S.Session_ID = SS.ID
                            INNER JOIN dbo.atbv_WorkOut_Exercises AS E
                                ON E.ID = S.Exercise_ID
                            GROUP BY SS.ID, E.Name
              </code></pre>
              </details>
            </td>
          </tr>
          <tr>
            <td><b>aviw_WorkOut_Session</b></td>
            <td>
              Dette viewet viser alle session feltene, og routine name.
            </td>
            <td>
              <details>
              <summary>
                <h4>Expand</h4>
              </summary> 
              <pre><code>
                SELECT
                    S.ID, S.PrimKey, S.Created, S.CreatedBy_ID, S.Updated, S.UpdatedBy_ID,
                    S.Routine_ID, S.StartTime, S.EndTime, R.Name AS RoutineName
                        FROM dbo.atbv_WorkOut_Session AS S
                            INNER JOIN dbo.atbl_WorkOut_Routines AS R
                                ON R.ID = S.Routine_ID
              </code></pre>
              </details>
            </td>
          </tr>
        </table>
      </details>
      <details open>
<summary>
  <h2>Sikkerhet</h2>
</summary>

I Omega 365 CTP håndteres autentisering sikkert. Brukere kan logge inn med Microsoft-konto eller SQL-login via e-post/telefonnummer og passord.
Ved Microsoft-login skjer autentiseringen gjennom Microsoft-systemer, som returnerer en token knyttet til brukeren i Omega 365 CTP. Ved SQL-login skjer autentisering via et API-kall som validerer bruker mot databasen.
To-faktor-autentisering (2FA) gir et ekstra sikkerhetslag. Rollebasert tilgangsstyring definerer hva hver bruker har tilgang til (moduler, apper og tabeller).

### Sikkerhet brukt i tabellene

#### Trigger
<pre><code>IF NOT EXISTS (SELECT 1
                FROM Inserted AS I
                INNER JOIN dbo.atbl_WorkOut_Session AS IC
                    ON IC.ID = I.ID
                WHERE NOT EXISTS (SELECT 1
                                    FROM dbo.sviw_System_MyPermissionsTables AS MPT
                                    WHERE MPT.TableName = N'atbl_WorkOut_Session'
                                        AND MPT.[ReadOnly] = 0
                                        AND MPT.AllowEdit  = 1))

BEGIN
    GOTO ContinueTransaction
END
</code></pre>

Denne sikkerheten finnes i alle tabellene knyttet til workout namespacet. Her blir en double not exists sjekk gjort mot sviw_System_MyPermissionsTables som inneholder tilganger knyttet til brukeren basert på roller.

#### Atbv
<pre><code>-- Appframe generated code: ------------------------------------------[autogenerated]--
SELECT t.[PrimKey], t.[CCTL], t.[ID], t.[Created], 
  t.[CreatedBy_ID], t.[Updated], t.[UpdatedBy_ID], t.[Routine_ID], 
  t.[StartTime], t.[EndTime]
    FROM dbo.atbl_WorkOut_Session AS t
 
-- Custom security check: ---------------------------------------------[customizable]--
WHERE EXISTS (SELECT 1
	        FROM dbo.sviw_System_MyPermissionsTables AS MPT
	        WHERE MPT.TableName = 'atbl_WorkOut_Session')
    AND t.CreatedBy_ID = dbo.sfnc_system_GetMyPersonID()
</code></pre>

I alle tabeller i Omega 365 CTP finnes det en lik atbv. Vi gir vanligvis ikke brukere tabell select tilganger, så all data som brukerene ser er via views. Atbver blir autogenerert i det man oppretter en tabell og default sikkerheten er en sjekk som sier 1=0. Dette gjør at all dataen blir sjult og er der sånn at
utviklerene blir tvingt til å tenke på sikkerheten. I min løsning er nesten all data begrenset per person så jeg gjør en sjekk mot CreatedBy_ID for å kun vise dataen til den spesifikke brukeren.

I Omega 365 CTP får ikke brukere direkte tilgang til tabeller. All visning skjer via views (`atbv_*`). Atbver blir generert automatisk når man opretter en tabell og default-sikkerheten skjuler all data (`1=0`), som tvinger utviklere til å tenke på sikkerheten. I løsningen min er dataen brukerspesifikk, jeg gjør en  filtrert på `CreatedBy_ID`.


</details>
    </li>
  </ul>

</details>
