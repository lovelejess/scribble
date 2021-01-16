---
title: "swiftui - webview wrapper"
date: 2020-01-16 10:57:14
---

### How to Create a WebView in SwiftUI

```
import Foundation
import UIKit
import SwiftUI
import WebKit

struct WebView: UIViewRepresentable {

    var url:String

    func makeUIView(context: Context) -> WKWebView {
        guard let url = URL(string: self.url) else {
            return WKWebView()
        }
        let request = URLRequest(url: url)
        let webView = WKWebView()
        webView.load(request)
        return webView
    }

    func updateUIView(_ uiView: WKWebView, context: Context) {
    }
}

```

example to use it: 


```
import Foundation
import SwiftUI

struct ContentView: View {
    var body: some View {
        WebView(url:"https://lovelejess.github.io/")
    }
}

```