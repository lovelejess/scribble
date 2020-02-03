---
title: "swift - error handling"
date: 2020-02-01 15:15:06
---

# Error Handling

There are four ways to handle errors in Swift:


### 1. Handle Errors with Do-Catch

    func readFileIntoStringDoCatch(fileName: String, fileExtension: String) {
        if let fileURL = Bundle.main.url(forResource: fileName, withExtension: fileExtension) {
            do {
                let content = try String(contentsOf: fileURL, encoding: .utf8)
                print(content)
            } catch {
                print("error handled immediately!")
            }
        }
    }

    readFileIntoStringDoCatch(fileName: "swift", fileExtension: "png")

### 2. Convert Error to Optional with try?

    func readFileIntoStringOptional(fileName: String, fileExtension: String) {
        if let fileURL = Bundle.main.url(forResource: fileName, withExtension: fileExtension) {
            if let content = try? String(contentsOf: fileURL, encoding: .utf8) {
                print(content)
            } else {
                print("could not read contents of file into string")
            }
        }
    }

    readFileIntoStringOptional(fileName: "swift", fileExtension: "png")


### 3. Ignore Error with try!

    func readFileIntoStringOrCrash(fileName: String, fileExtension: String) {
        if let fileURL = Bundle.main.url(forResource: fileName, withExtension: fileExtension) {
            let content = try! String(contentsOf: fileURL, encoding: .utf8)
            print(content)
        }
    }

    readFileIntoStringOrCrash(fileName: "swift", fileExtension: "txt")

### 4. Propagate Error

    func readFileIntoStringOrPropagate(fileName: String, fileExtension: String) throws {
        if let fileURL = Bundle.main.url(forResource: fileName, withExtension: fileExtension) {
            let content = try String(contentsOf: fileURL, encoding: .utf8)
            print(content)
        }
    }

    do {
        try readFileIntoStringOrPropagate(fileName: "swift", fileExtension: "png")
    } catch {
        print("the error was propagated to me, and I handled it!")
    }


## Custom Error Handling

Create custom errors via enums that are of type `Error`

    enum PurchaseError: Error {
        case invalidAddress
        case cardRejected
        case invalidPromo
        case invalidCardInformation
    }

Use associated values to add helpful debugging information.

    enum PurchaseError: Error {
        case invalidAddress
        case cardRejected
        case invalidPromo(String)
        case invalidCardInformation(String)
    }

    do {
        try attemptPurchase(withPromoCode: "FREE")
    } catch let error as PurchaseError {
        switch error {
        case let .invalidPromo(code): /* extract the associated value */
            print("Invalid Promo Code: \(code)")
        default:
            print(error)
            break
        }
    } catch {
        print(error)
    }

### LocalizedError and CustomNSError    
    
Add extra debugging information to a custom error by also implementing the **LocalizedError** and **CustomNSError** protocols.


    extension PurchaseError: LocalizedError {   
        var failureReason: String? {
            switch self {
            case .invalidAddress:
                return NSLocalizedString("Address invalid or contained empty fields", comment: "")
            case .cardRejected:
                return NSLocalizedString("Card Rejected", comment: "")
            case let .invalidPromo(code):
                return NSLocalizedString("Invalid Promo Code: \(code)", comment: "")
            case .invalidCardInformation(String):
                return NSLocalizedString("Invalid Card information: \(info)", comment: "")
            }
        }
    }


    extension PurchaseError: CustomNSError {
        // domain and error code...

        /// The user-info dictionary.
        var errorUserInfo: [String: Any] {
            switch self {
            case let .invalidPromo(code):
                return [
                    "promoCode": code,
                    "dateExpired":getExpirationDate(code) ?? "n/a"
                ]
            case let .invalidCardInformation(info):
                return [
                    "cardInformation": info
                ]
            default:
                return [:]
            }
        }
    }
