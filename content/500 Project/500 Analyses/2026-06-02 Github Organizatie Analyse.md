---
tags:
  - analyse
created: "2026-06-02T14:01:00"
---
# 2026-06-02 Github Organizatie Analyse

Het analyseren van de github organisatie in authorization en authentication. 

Analysis done by:
- #user/martijn 

## Scope

**Included:**
- The [github organisation](https://github.com/Avans-2-4).
- The [project repository](https://github.com/Avans-2-4/Appointment-Scheduling-Audit).
- The [obsidian repository](https://github.com/Avans-2-4/Documentatie-Avans-2-4).
**Not Included:**
- The "[softwaredesign-en-kwaliteit](https://github.com/Avans-2-4/Softwaredesign-en-kwaliteit-Avans2-4LU2)" Repositorystarting on 
- The [Quartz framework](https://github.com/jackyzha0/quartz) (the way we host our markdown on a website)

## Relevante eisen

De Git omgeving moet veilig zijn. We willen beveiligd zijn op de mogelijkheid van insider threats. Hierbij hebben we de volgende globale eisen opgesteld:
1. Alleen administrators mogen dingen verwijderen
2. Alerts bij aanpassingen aan CI/CD files
3. beveiliging tegen supply chain attacks
4. 2 factor authentication is required
5. Het volgen van best practices 

## Analysis

**Methodes / stappen:**
- Organization:
	- Ik lees door de instellingen om te onderzoeken hoe het momenteel is ingesteld.
	- Ik onderzoek wat de beste instellingenzijn voor onze eisen.
- Repository:
	- Ik lees door de instellingen om te onderzoeken hoe het momenteel is ingesteld.
	- Ik onderzoek wat de beste instellingenzijn voor onze eisen.

## Findings

Ik denk dat ik vrijwel alle security en best practices heb gevonden.

### Improvements

*Voor de organisation veranderingen worden de github issues gehangen aan de [.github](https://github.com/Avans-2-4/.github) repository aangezien die issues niet over een project gaan maar over de org, en de .github repo is voor de organisation informatie.*

*De requirements kolom is grotendeels leeg omdat er niet echt requirements hangen aan simpele taken van "doe dit", behalve dat je het moet doen.*

| Name                       | Issue Description                                                                                                                                                                                                                                                                                                       | Requirements                                                                                           | Importance / Ignored | Justification                                                                                                                                                                                                                                     | Issue Link                                                                                                                                                               | Feedback klasgenoot |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------- |
| Delete repository rights   | Allow members to delete or transfer repositories for this organization needs to be disabled<br>[org settings](https://github.com/organizations/Avans-2-4/settings) > member privilages > Repository deletion and transfer > disable it                                                                                  |                                                                                                        | High                 | Alleen admins mogen dit soort acties doen                                                                                                                                                                                                         | [link](https://github.com/Avans-2-4/.github/issues/7)                                                                                                                    |                     |
| Immutable Releases         | All repositories should have immutable releases.<br>[org settings](https://github.com/organizations/Avans-2-4/settings) > repository > general > releases > set it to `all repositories`                                                                                                                                |                                                                                                        | Medium               | Zonder dit heb je kans op mogelijke insider threat acties.                                                                                                                                                                                        | [link](https://github.com/Avans-2-4/.github/issues/6)                                                                                                                    |                     |
| organization ruleset       | create a ruleset to define how collaborators interact with the repositories. To enable this, [upgrade the organisation to premium ](https://github.com/organizations/Avans-2-4/billing/plans)<br><br>[org settings](https://github.com/organizations/Avans-2-4/settings) > repository > rulesets > create a ruleset<br> |                                                                                                        | Ignored              | We gaan niet betalen voor premium, maar dit zou wel de beste optie zijn                                                                                                                                                                           | [link](https://github.com/Avans-2-4/.github/issues/5)                                                                                                                    |                     |
| restrictive github actions | Only allow custom CI/CD and approved CI/CD workflows.<br>[org settings](https://github.com/organizations/Avans-2-4/settings) > actions > general > policies > zet het om naar `Allow Avans-2-4, and select non-Avans-2-4, actions and reusable workflows` en sta alleen github en verified creators toe                 |                                                                                                        | Ignored              | Dit zorgt er voor dat je niet zomaar een random CI/CD workflow kan pakken en runnen. Dit is veiliger, we vertrouwen nu alleen trusted sources.<br><br>Maar dit laten we vallen omdat we geen tijd hebben om alle actions te reviewen en approven. | [link](https://github.com/Avans-2-4/.github/issues/4)                                                                                                                    |                     |
| github action SHA          | forceer workflow aanpassingen verbonden te zijn aan een commit SHA<br><br>[org settings](https://github.com/organizations/Avans-2-4/settings) > actions > general > policies > enable `Require actions to be pinned to a full-length commit SHA`                                                                        |                                                                                                        | Ignored              | Hiermee zorgen we dat we kunnen terugkijken als iemand de CI/CD aanpast. Inside threat protectie.<br><br>Maar dit laten we vallen omdat we geen tijd hebben om alle actions te reviewen en approven.                                              | [link](https://github.com/Avans-2-4/.github/issues/3)                                                                                                                    |                     |
| 2FA                        | Forceer dat iedereen 2fa aan heeft staan<br><br>[org settings](https://github.com/organizations/Avans-2-4/settings) > authentication security > two-factor authentication > enable `Require two-factor authentication for everyone in the SBOMboclat Avans 2-4 organization.` en `Only allow secure two-factor methods` |                                                                                                        | High                 | Dit is gewoon een best practice.                                                                                                                                                                                                                  | [link](https://github.com/Avans-2-4/.github/issues/2)                                                                                                                    |                     |
| suggest pull requests      | Whenever there are new changes available in the base branch, present an “update branch” option in the pull request.<br><br>repo settings > pull request > enable `Always suggest updating pull request branches`                                                                                                        |                                                                                                        | Low                  | best practice om je branch up-to-date te hebben.                                                                                                                                                                                                  | [project repo](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/11), [documentation repo](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/35) |                     |
| branch protection rules    | configure branch protection rules<br><br>repo settings > branches                                                                                                                                                                                                                                                       | directe pushes naar main en develop mag niet.<br>block force pushes.<br>require a pull before merging. | High                 | best practice                                                                                                                                                                                                                                     | [project repo](https://github.com/Avans-2-4/Documentatie-Avans-2-4/issues/36), [documentation repo](https://github.com/Avans-2-4/Appointment-Scheduling-Audit/issues/12) |                     |
| notifications              | some external notification service that sends a message in the discord whenever CI/CD is changed (the `.github/workflows` folder)                                                                                                                                                                                       |                                                                                                        | Medium               | we willen genotified worden als op 1 van de repo's de CI/CD aangepast wordt, want dit is een single oint of failure in een insider threat scenario                                                                                                | [link](https://github.com/Avans-2-4/.github/issues/1)                                                                                                                    |                     |

*Ignored betekent dat we erkennen dat het verbeterd zou moeten worden, maar dat we er niet aan toe gaan komen omdat het geen prioriteit is. Wanneer we dit doen geven we hiervoor een rede. Ook open we hiervoor nogsteeds een issue op github, die we direct weer sluiten.*

### Algemene feedback klasgenoot