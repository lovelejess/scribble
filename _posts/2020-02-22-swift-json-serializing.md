---
title: "swift - serializing json"
date: 2020-02-22 11:30:53
---

## Serializing JSON

Swift provides an easy way to serialize JSON to Swift objects via `JSONEncoder`.

    import Foundation
    import CoreFoundation

    // create a Codable struct called "POST" with the correct properties
    struct Post: Codable {
        let userId: Int
        let title: String
        let body: String
    }   

    // create an instance of the Post struct with your own values
    let post = Post(userId: 123,title: "Title",  body: "body")

    // create a URLRequest by passing in the URL
    var request = URLRequest(url: URL(string: "https://jsonplaceholder.typicode.com/posts")!)

    // set the HTTP method to POST
    request.httpMethod = "POST"

    // set the HTTP body to the encoded "Post" struct
    request.httpBody = try! JSONEncoder().encode(post)

    // set the appropriate HTTP header fields
    request.addValue("application/json", forHTTPHeaderField: "Content-Type")

    // HACK: this line allows the workspace or an Xcode playground to execute the request, but is not needed in a real app
    let runLoop = CFRunLoopGetCurrent()
    // task for making the request
    let task = URLSession.shared.dataTask(with: request) {data, response, error in
        print(String(data: data!, encoding: .utf8))
        // also not necessary in a real app
        CFRunLoopStop(runLoop)
    }
    task.resume()
    // not necessary
    CFRunLoopRun()


    //prints: Optional("{\n \"userId\": 123,\n \"body\": \"body\",\n \"title\": \"Title\",\n \"id\": 101\n}")


### Generic Function for POST Requests

    class func taskForPOSTRequest<RequestType: Encodable, ResponseType: Decodable>(url: URL, responseType: ResponseType.Type, body: RequestType, completion: @escaping (ResponseType?, Error?) -> Void) {
        var request = URLRequest(url: url)
        request.httpMethod = "POST"
        request.httpBody = try! JSONEncoder().encode(body)
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        let task = URLSession.shared.dataTask(with: request) { data, response, error in
            guard let data = data else {
                DispatchQueue.main.async {
                    completion(nil, error)
                }
                return
            }
            let decoder = JSONDecoder()
            do {
                let responseObject = try decoder.decode(ResponseType.self, from: data)
                DispatchQueue.main.async {
                    completion(responseObject, nil)
                }
            } catch {
                DispatchQueue.main.async {
                    completion(nil, error)
                }
            }
        }
        task.resume()
    }

#### Example

    Here's an example of how one would call the above method:

    class func login(username: String, password: String, completion: @escaping (Bool, Error?) -> Void) {
        let body = LoginRequest(username: username, password: password, requestToken: Auth.requestToken)
        
        taskForPOSTRequest(url: Endpoints.login.url, responseType: RequestTokenResponse.self, body: body) { (response, error) in
            if let response = response {
                Auth.requestToken = response.requestToken
                completion(true, nil)
            }
            else {
                completion(false, error)
            }
        }
    }