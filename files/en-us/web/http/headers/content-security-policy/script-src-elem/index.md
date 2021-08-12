---
title: 'CSP: script-src-elem'
slug: Web/HTTP/Headers/Content-Security-Policy/script-src-elem
tags:
  - CSP
  - Content
  - Content-Security-Policy
  - Directive
  - HTTP
  - Reference
  - Script
  - Security
  - script-src
  - source
browser-compat: http.headers.csp.Content-Security-Policy.script-src-elem
---
<div>{{HTTPSidebar}}</div>

<p>The HTTP {{HTTPHeader("Content-Security-Policy")}} (CSP) <strong><code>script-src-elem</code></strong> directive specifies valid sources for JavaScript {{HTMLElement("script")}} elements, but not inline script event handlers like <code>onclick</code>.</p>

<table class="properties">
 <tbody>
  <tr>
   <th scope="row">CSP version</th>
   <td>3</td>
  </tr>
  <tr>
   <th scope="row">Directive type</th>
   <td>{{Glossary("Fetch directive")}}</td>
  </tr>
  <tr>
   <th scope="row">{{CSP("default-src")}} fallback</th>
   <td>Yes. If this directive is absent, the user agent will look for the {{CSP("script-src")}} directive, and if both of them are absent, fallback to <code>default-src</code> directive.</td>
  </tr>
 </tbody>
</table>

<h2 id="Syntax">Syntax</h2>

<p>One or more sources can be allowed for the <code>script-src-elem</code> policy:</p>

<pre class="brush: html">Content-Security-Policy: script-src-elem &lt;source&gt;;
Content-Security-Policy: script-src-elem &lt;source&gt; &lt;source&gt;;
</pre>

<p><code>script-src-elem</code> can be used in conjunction with {{CSP("script-src")}}:</p>

<pre class="brush: html">Content-Security-Policy: script-src &lt;source&gt;;
Content-Security-Policy: script-src-elem &lt;source&gt;;
</pre>

<h3 id="Sources">Sources</h3>

<dl>
  <dt><code>&lt;host-source&gt;</code></dt>
  <dd>Internet hosts by name or IP address, as well as an optional <a href="/en-US/docs/Learn/Common_questions/What_is_a_URL">URL scheme</a> and/or port number. The site's address may include an optional leading wildcard (the asterisk character, <code>'*'</code>), and you may use a wildcard (again, <code>'*'</code>) as the port number, indicating that all legal ports are valid for the source.<br>
    Examples:
    <ul>
      <li><code>http://*.example.com</code>: Matches all attempts to load from any subdomain of example.com using the <code>http:</code> URL scheme.</li>
      <li><code>mail.example.com:443</code>: Matches all attempts to access port 443 on mail.example.com.</li>
      <li><code>https://store.example.com</code>: Matches all attempts to access store.example.com using <code>https:</code>.</li>
      <li><code>*.example.com</code>: Matches all attempts to load from any subdomain of example.com using the current protocol.</li>
    </ul>
  </dd>

  <dt></ode>&lt;scheme-source&gt;</code></dt>
  <dd>A scheme such as <code>http:</code> or <code>https:</code>. The colon is required. Unlike other values below, single quotes shouldn't be used. You can also specify data schemes (not recommended).
    <ul>
      <li><code>data:</code> Allows <a href="/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs"><code>data:</code> URIs</a> to be used as a content source.<em> This is insecure; an attacker can also inject arbitrary data: URIs. Use this sparingly and definitely not for scripts.</em></li>
      <li><code>mediastream:</code> Allows <a href="/en-US/docs/Web/API/Media_Streams_API"><code>mediastream:</code> URIs</a> to be used as a content source.</li>
      <li><code>blob:</code> Allows <a href="/en-US/docs/Web/API/Blob"><code>blob:</code> URIs</a> to be used as a content source.</li>
      <li><code>filesystem:</code> Allows <a href="/en-US/docs/Web/API/FileSystem"><code>filesystem:</code> URIs</a> to be used as a content source.</li>
    </ul>
  </dd>

  <dt><code>'self'</code></dt>
  <dd>Refers to the origin from which the protected document is being served, including the same URL scheme and port number. You must include the single quotes. Some browsers specifically exclude <code>blob</code> and <code>filesystem</code> from source directives. Sites needing to allow these content types can specify them using the Data attribute.</dd>

  <dt><code>'unsafe-eval'</code></dt>
  <dd>Allows the use of <code>eval()</code> and similar methods for creating code from strings. You must include the single quotes.</dd>

  <dt><code>'unsafe-hashes'</code></dt>
  <dd>Allows enabling specific inline <a href="/en-US/docs/Web/Events/Event_handlers">event handlers</a>. If you only need to allow inline event handlers and not inline {{HTMLElement("script")}} elements or <code>javascript:</code> URLs, this is a safer method than using the <code>unsafe-inline</code> expression.</dd>

  <dt><code>'unsafe-inline'</code></dt>
  <dd>Allows the use of inline resources, such as inline {{HTMLElement("script")}} elements, <code>javascript:</code> URLs, and inline event handlers. The single quotes are required.</dd>

  <dt><code>'none'</code></dt>
  <dd>Refers to the empty set; that is, no URLs match. The single quotes are required.</dd>

  <dt><code>'nonce-&lt;base64-value&gt;'</code></dt>
  <dd>An allow-list for specific inline scripts using a cryptographic nonce (number used once). The server must generate a unique nonce value each time it transmits a policy. It is critical to provide an unguessable nonce, as bypassing a resource's policy is otherwise trivial. See <a href="/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src#unsafe_inline_script">unsafe inline script</a> for an example. Specifying nonce makes a modern browser ignore <code>'unsafe-inline'</code> which could still be set for older browsers without nonce support.
    <div class="notecard note">
      <p><strong>Note:</strong> The CSP <code>nonce</code> source can only be applied to <em>nonceable</em> elements (e.g., as the {{HTMLElement("img")}} element has no <code>nonce</code> attribute, there is no way to associate it with this CSP source).</p>
    </div>
  </dd>

  <dt><code>'&lt;hash-algorithm&gt;-&lt;base64-value&gt;'</code></dt>
  <dd>A sha256, sha384 or sha512 hash of scripts or styles. The use of this source consists of two portions separated by a dash: the encryption algorithm used to create the hash and the base64-encoded hash of the script or style. When generating the hash, don't include the &lt;script&gt; or &lt;style&gt; tags and note that capitalization and whitespace matter, including leading or trailing whitespace. See <a href="/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src#unsafe_inline_script">unsafe inline script</a> for an example. In CSP 2.0, this is applied only to inline scripts. CSP 3.0 allows it in the case of <code>script-src</code> for external scripts.</dd>

  <dt><code>'strict-dynamic'</code></dt>
  <dd>The <code>strict-dynamic</code> source expression specifies that the trust explicitly given to a script present in the markup, by accompanying it with a nonce or a hash, shall be propagated to all the scripts loaded by that root script. At the same time, any allow-list or source expressions such as <code>'self'</code> or <code>'unsafe-inline'</code> are ignored. See <a href="/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src#strict-dynamic">script-src</a> for an example.</dd>

  <dt><code>'report-sample'</code></dt>
  <dd>Requires a sample of the violating code to be included in the violation report.</dd>
</dl>

<h2 id="Examples">Examples</h2>

<h3 id="Fallback_to_script-src">Fallback to script-src</h3>

<p>If <code>script-src-elem</code> is absent, User Agent falls back to the {{CSP("script-src")}} directive, and if that is absent as well, to {{CSP("default-src")}}.</p>

<h2 id="Specifications">Specifications</h2>

{{Specifications}}

<h2 id="Browser_compatibility">Browser compatibility</h2>

<p>{{Compat}}</p>

<h2 id="See_also">See also</h2>

<ul>
 <li>{{HTTPHeader("Content-Security-Policy")}}</li>
 <li>{{HTMLElement("script")}}</li>
 <li>{{CSP("script-src")}}</li>
 <li>{{CSP("script-src-attr")}}</li>
</ul>