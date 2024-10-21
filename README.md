# DynamicViewsAndroid
```
package com.example.exampleapp

import android.os.Bundle
import android.view.View
import com.example.exampleapp.databinding.ActivityMainBinding
import com.example.exampleapp.util.showToast
import android.webkit.WebView
import android.webkit.WebViewClient
import androidx.appcompat.app.AppCompatActivity
import android.widget.LinearLayout
import android.widget.RadioButton

class MainActivity : AppCompatActivity() {
    private lateinit var linearLayout: LinearLayout
    lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root) // Make sure this is your layout file

        linearLayout = findViewById(R.id.linearLayout)
        val mathFormulas ="HTML DATA"
        val htmlData = getScriptStringServer(mathFormulas)

        for (i in 1..4) {
            // Create a horizontal LinearLayout to hold the RadioButton and WebView
            val horizontalLayout = LinearLayout(this)
            horizontalLayout.layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,500
            )
            horizontalLayout.orientation = LinearLayout.HORIZONTAL
            val radioButton = RadioButton(this)
            radioButton.layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.WRAP_CONTENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
            )
            val webView = WebView(this)
            val params = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.MATCH_PARENT)
            params.weight = 1f
            webView.layoutParams = params
            webView.settings.javaScriptEnabled = true
            webView.webViewClient = WebViewClient() // To open links in the WebView
            webView.loadDataWithBaseURL(null, htmlData, "text/html", "UTF-8", null)
            webView.setWebViewClient(object : WebViewClient() {
                override fun onPageFinished(view: WebView?, url: String?) {
                    showToast("Page loaded in WebView $i")
                }
            })
            horizontalLayout.addView(radioButton)
            horizontalLayout.addView(webView)

            linearLayout.addView(horizontalLayout)
        }
    }
}

fun getScriptStringServer(text: String): String {
    return """
        <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
          <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
          <script>
            window.addEventListener('load', function() {
              MathJax.typesetPromise().then(function() {
                document.body.style.visibility = 'visible';
              }).catch(function(err) {
                console.error('MathJax typesetting error:', err);
              });
            });
          </script>
        </head>
        <body style='visibility:hidden;'>
          $text
        </body>
        </html>
    """.trimIndent()
}
```
