---
title: "swift - deserializing json"
date: 2020-02-05 18:35:13
---

## Deserializing JSON

Swift provides an easy way to deserialize JSON to Swift objects via `JSONDecoder`.

    import Foundation

    var json = """
    {
    "climberId": 42,
    "name": "Jess",
    "type": "boulder",
    "grade": 6,
    }
    """.data(using: .utf8)!

    // create a struct called "Climber" that conforms to Codable
    // the struct should have properties with matching names and data types to the JSON

    struct Climber: Codable {
        let climberId: Int
        let name: String
        let type: String
        let grade: Int
    }

    // create a JSON decoder
    let decoder = JSONDecoder()
    
    // decode the JSON into your model object and print the result
    do {
        let climber = try decoder.decode(Climber.self, from: json)
        print(climber) // prints Climber(climberId: 42, name: "Jess", type: "boulder", grade: 6)
    } catch {
        print(error)
    }


## Mapping JSON Keys to Swift 

What if the JSON keys aren't easily mappable to swift? That's ok! Use `CodingKey` protocol to map them as you wish.

    import Foundation

    var json = """
    {
        "climber id": 42,
        "name": "Jess",
        "type_of_climb": "boulder",
        "grade": 6,
    }
    """.data(using: .utf8)!

    // create a struct called "Climber" that conforms to Codable
    // the struct should have properties with matching names and data types to the JSON

    struct Climber: Codable {
        let climberId: Int
        let name: String
        let type: String
        let grade: Int

        // add CodingKeys enum here
        enum CodingKeys: String, CodingKey {
            case climberId = "climber id"
            case name = "name"
            case type = "type_of_climb"
            case grade = "grade" 
        }
    }

    // create a JSON decoder
    let decoder = JSONDecoder()
    
    // decode the JSON into your model object and print the result
    do {
        let climber = try decoder.decode(Climber.self, from: json)
        print(climber) // prints Climber(climberId: 42, name: "Jess", type: "boulder", grade: 6)
    } catch {
        print(error)
    }



## Mapping Arrays


    import Foundation

    var json = """
    [
        {
            "climber id": 42,
            "name": "Jess",
            "type_of_climb": "boulder",
            "grade": 6,
            "ascent": ["01/05/2020", "02/10/2020", "02/12/2020"]
        },
        {
            "climber id": 42,
            "name": "Jess",
            "type_of_climb": "boulder",
            "grade": 8,
            "ascent": []
        }
    ]
    """.data(using: .utf8)!

    struct Climber: Codable {
        let climberId: Int
        let name: String
        let type: String
        let grade: Int
        let ascent: [String]

        // add CodingKeys enum here
        enum CodingKeys: String, CodingKey {
            case climberId = "climber id"
            case name = "name"
            case type = "type_of_climb"
            case grade
            case ascent
        }
    }

    // create a JSON decoder
    let decoder = JSONDecoder()

    // decode the JSON into your model object and print the result
    do {
        let climber = try decoder.decode([Climber].self, from: json)
        print(climber) // prints [workspace.Climber(climberId: 42, name: "Jess", type: "boulder", grade: 6, ascent: ["01/05/2020", "02/10/2020", "02/12/2020"]), workspace.Climber(climberId: 42, name: "Jess", type: "boulder", grade: 8, ascent: [])]
    } catch {
        print(error)
    }
    

## Nested JSON

What if the JSON contains nested fields? No Problem! Swifth handles it as well! Create a typed struct that conforms to the `Codable` protocol and reference that type in the parent struct

    import Foundation

    var json = """
    {
            "climber id": 42,
            "name": "Jess",
            "type_of_climb": "boulder",
            "grade": 6,
            "ascent": {
                "date": "01/05/2020",
                "type": "redpoint"
            }
        }
    """.data(using: .utf8)!

    struct Ascent: Codable {
        let date: String
        let type: String
    }

    struct Climber: Codable {
        let climberId: Int
        let name: String
        let type: String
        let grade: Int
        let ascent: Ascent

        // add CodingKeys enum here
        enum CodingKeys: String, CodingKey {
            case climberId = "climber id"
            case name = "name"
            case type = "type_of_climb"
            case grade = "grade"
            case ascent
        }
    }

    // create a JSON decoder
    let decoder = JSONDecoder()

    // decode the JSON into your model object and print the result
    do {
        let climber = try decoder.decode(Climber.self, from: json)
        print(climber) // prints Climber(climberId: 42, name: "Jess", type: "boulder", grade: 6, ascent: workspace.Ascent(date: "01/05/2020", type: "redpoint"))
    } catch {
        print(error)
    }