# Stock Appreciation Rights

```swift
struct WEVESTRAanleverData {

    // WEVESTR zal de hieronder beschreven data aanleveren in een JSON-object naar een server. De server creërt de documenten en verschaft een url naar de locatie met de documenten. Verder afstemmen met WEVESTR developers hiervoor is nodig. Na de data-beschrijving (in Apple swift) volgt een voorbeeld JSON object.

    // Een struct is een product-type (dit, dit, én, dit)
    // Een enum is een sum-type (dit, dit, óf dit)

    // Optie: ofwel 1 taal naar keuze, ofwel altijd Nederlands en Engels uitdraaien.
    let language:LanguageForDocuments = .dutch
    enum LanguageForDocuments {
        case dutch
        case english
        case englishAndDutch
    }

    struct TranslatedString {
        let dutch:String
        let english:String
    }

    // Uitgevende BV NAW-gegevens en kvk-nummer
    let bv_name:String
    let bv_adres_straat:String
    let bv_adres_nummer:String
    let bv_adres_toevoeging:String
    let bv_adres_postcode:String
    let bv_woonplaats:Woonplaats
    let bv_kvk:String

    // Gegevens van de vertegenwoordigers van de BV. Veelal zal dat een of twee natuurlijk personen zijn. Van hun zijn naam en optioneel een titel vereist.
    let bv_representatives: [Representative]
    struct Representative:Codable, Hashable {
        let name:TranslatedString
        let title:TranslatedString?
        let date:Date?
    }

    // Datum van de uitgifte van de SARs
    let date:Date?

    // De BV kan kiezen of er een correctie wordt toegepast in verband met verwatering of dividenduitkering. Als dit niet het geval is, kan de waarde van de SAR door de BV worden beïnvloed door uitgifte nieuwe aandelen of dividenduitkering. Als er een correctie optreedt, zal de waarde van de SAR daardoor niet beïnvloed worden.
    let correctie_op_waardering_in_verband_met_verwatering_of_dividenduitkering: Bool

    // De BV moet aangeven op welke manier de waarde van de SAR wordt bepaald.
    let method_to_determine_value: MethodToDetermineValueOfSAR

    // De BV moet aangeven wie kan besluiten tot SAR-uitkering. Dit zal veelal het bestuur en/of de algemene vergadering zijn, maar een andere keuze is ook mogelijk.
    let wie_kan_besluiten_tot_SAR_Uitkering: WieKanBesluiten

    // De BV moet aangeven wie kan besluiten tot beëindiging van de SAR. Dit zal veelal het bestuur en/of de algemene vergadering zijn, maar een andere keuze is ook mogelijk.
    let wie_kan_besluiten_tot_SAR_Beëindiging: WieKanBesluiten

    // Per recipient moeten NAW gegevens, aantal set SARs (en op welke soort en aanduiding), en optioneel een vesting worden aangegeven. Met een set bedoel ik een verzameling van unieke elementen.
    let recipients: [Recipient<SoortAanduidingESOP>] = []

    // Er zijn verschillende manieren om de waarde vast te stellen. Hieronder staan een aantal veelgebruikte methoden. In sommige gevallen moet ook een factor worden ingevoerd. Een custom keuze kan ook.
    enum MethodToDetermineValueOfSAR {
        case factorxNettoWinst(factor:Double)
        case factorxEBIT(factor:Double)
        case factorxEBITDA(factor:Double)
        case factorxJaaromzet(factor:Double)
        case factorxBrutomarge(factor:Double)
        case rentabiliteitswaarde
        case intrinsiekeWaarde
        case dcf
        case custom(TranslatedString)
    }

    enum WieKanBesluiten{
        case bestuur
        case algemeneVergadering
        case bestuurOfAlgemeneVergadering
        case bestuurEnAlgemeneVergadering
        case custom(TranslatedString)
    }

    struct Recipient {
        let person_names:[String]
        let person_lastname:String
        let title:TranslatedString?
        let date:Date?
        let sars:Set<StockAppreciationRight>
    }

    struct StockAppreciationRight {
        let amount:Int
        let aanduiding: SoortAanduiding
        let vesting:Vesting?
    }

    enum SoortAanduiding {
        case ordinary
        case special(TranslatedString)
    }

    enum Vesting {
        case jaren(Int)
    }
}
```

Voorbeeld valide JSON:
```json
{
  "bv_adres_straat" : "Demostraat",
  "recipients" : [
    {
      "person" : {
        "theNetherlands" : {
          "_0" : {
            "identification" : {
              "bsn" : ""
            },
            "achternaam" : "van Wijk",
            "geboorteplaats" : "West Maas en Waal",
            "geslacht" : "female",
            "geboorteland" : "Nederland",
            "date" : {
              "birth" : -296030800.49026406
            },
            "adres" : {
              "theNetherlands" : {
                "_0" : {
                  "adres" : {
                    "_0" : {
                      "straat" : "",
                      "toevoeging" : "",
                      "postcode" : {
                        "nummers" : {
                          "first" : 1,
                          "third" : 1,
                          "fourth" : 1,
                          "second" : 1
                        },
                        "letters" : {
                          "first" : "A",
                          "second" : "A"
                        }
                      },
                      "nummer" : "",
                      "plaats" : "Amsterdam"
                    }
                  }
                }
              }
            },
            "voornamen" : [
              "Belle",
              "Belle"
            ],
            "roepnaam" : "Belle"
          }
        }
      },
      "sars" : [
        {
          "amount" : 10,
          "aanduiding" : {
            "custom" : {
              "nominal" : {
                "euro" : {
                  "_0" : {
                    "cash" : {
                      "_0" : 1
                    }
                  }
                }
              },
              "voor_de_uitgifte_in_de_statuten_is_bepaald_dat_zij_kunnen_worden_ingetrokken_met_terugbetaling" : false,
              "translatedDescription" : {
                "dutch" : "Serie A",
                "english" : "Serie A"
              }
            }
          },
          "vesting" : {
            "jaren" : {
              "_0" : 5
            }
          }
        },
        {
          "amount" : 10,
          "aanduiding" : {
            "ordinary" : {

            }
          }
        },
        {
          "amount" : 10,
          "aanduiding" : {
            "ordinary" : {

            }
          },
          "vesting" : {
            "jaren" : {
              "_0" : 5
            }
          }
        },
        {
          "amount" : 10,
          "aanduiding" : {
            "custom" : {
              "nominal" : {
                "euro" : {
                  "_0" : {
                    "cash" : {
                      "_0" : 1
                    }
                  }
                }
              },
              "voor_de_uitgifte_in_de_statuten_is_bepaald_dat_zij_kunnen_worden_ingetrokken_met_terugbetaling" : false,
              "translatedDescription" : {
                "dutch" : "Serie A",
                "english" : "Serie A"
              }
            }
          }
        }
      ],
      "title" : {
        "dutch" : "Marketing Directeur",
        "english" : "Marketing Director"
      },
      "date" : 682276399.50838494
    }
  ],
  "bv_name" : "WEVESTR BV",
  "bv_kvk" : "12345678",
  "correctie_op_waardering_in_verband_met_verwatering_of_dividenduitkering" : true,
  "wie_kan_besluiten_tot_SAR_Beëindiging" : {
    "bestuur" : {

    }
  },
  "wie_kan_besluiten_tot_SAR_Uitkering" : {
    "bestuur" : {

    }
  },
  "date" : 682276399.50838494,
  "bv_adres_nummer" : "1",
  "language" : {
    "dutch" : {

    }
  },
  "bv_representatives" : [
    {
      "name" : {
        "dutch" : "Coen",
        "english" : "Coen"
      },
      "title" : {
        "dutch" : "Representative",
        "english" : "Representative"
      },
      "date" : 682276399.50838494
    },
    {
      "name" : {
        "dutch" : "Robin",
        "english" : "Robin"
      }
    },
    {
      "name" : {
        "dutch" : "Floris",
        "english" : "Floris"
      },
      "date" : 682276399.50838494
    }
  ],
  "method_to_determine_value" : {
    "factorxNettoWinst" : {
      "factor" : 1
    }
  },
  "bv_adres_toevoeging" : "",
  "bv_woonplaats" : "Amsterdam",
  "bv_adres_postcode" : "1234AB"
}
```
