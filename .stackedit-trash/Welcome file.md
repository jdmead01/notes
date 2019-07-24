---


---

<h2 id="new-node-project">New Node Project</h2>
<pre class=" language-console"><code class="prism  language-console">mkdir {{project_name}}
cd {{project_name}}
mkdir static static/images static/css views
npm init -y
npm install express ejs express-session body-parser express-flash mongoose moment mongoose socket.io
code .
</code></pre>
<h5 id="create-server.js">create server.js</h5>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> express <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"express"</span><span class="token punctuation">)</span>
<span class="token keyword">const</span> bp <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"body-parser"</span><span class="token punctuation">)</span>
<span class="token keyword">const</span> path <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"path"</span><span class="token punctuation">)</span>
<span class="token keyword">const</span> session <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"express-session"</span><span class="token punctuation">)</span>
<span class="token keyword">const</span> flash <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"express-flash"</span><span class="token punctuation">)</span>
<span class="token keyword">var</span> app <span class="token operator">=</span> <span class="token function">express</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span>bp<span class="token punctuation">.</span><span class="token function">urlencoded</span><span class="token punctuation">(</span><span class="token punctuation">{</span>extended<span class="token punctuation">:</span> <span class="token boolean">true</span><span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span>express<span class="token punctuation">.</span><span class="token keyword">static</span><span class="token punctuation">(</span>path<span class="token punctuation">.</span><span class="token function">join</span><span class="token punctuation">(</span>__dirname<span class="token punctuation">,</span> <span class="token string">'./static'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span><span class="token function">session</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
    secret<span class="token punctuation">:</span> <span class="token string">'quotes'</span><span class="token punctuation">,</span>
    resave<span class="token punctuation">:</span> <span class="token boolean">false</span><span class="token punctuation">,</span>
    saveUninitialized<span class="token punctuation">:</span> <span class="token boolean">true</span><span class="token punctuation">,</span>
    cookie<span class="token punctuation">:</span> <span class="token punctuation">{</span> maxAge<span class="token punctuation">:</span> <span class="token number">60000</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span><span class="token function">flash</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>

app<span class="token punctuation">.</span><span class="token keyword">set</span><span class="token punctuation">(</span><span class="token string">'views'</span><span class="token punctuation">,</span> path<span class="token punctuation">.</span><span class="token function">join</span><span class="token punctuation">(</span>__dirname<span class="token punctuation">,</span> <span class="token string">'./views'</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
app<span class="token punctuation">.</span><span class="token keyword">set</span><span class="token punctuation">(</span><span class="token string">'view engine'</span><span class="token punctuation">,</span> <span class="token string">'ejs'</span><span class="token punctuation">)</span>
  
<span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'./routes'</span><span class="token punctuation">)</span><span class="token punctuation">(</span>app<span class="token punctuation">)</span>

app<span class="token punctuation">.</span><span class="token function">listen</span><span class="token punctuation">(</span><span class="token number">8000</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token operator">=&gt;</span><span class="token punctuation">{</span>
    <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span>
    <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"listening on port 8000..."</span><span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span>
</code></pre>
<h5 id="create-controller.js">create controller.js</h5>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> User <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"./models"</span><span class="token punctuation">)</span>

module<span class="token punctuation">.</span>exports <span class="token operator">=</span> <span class="token punctuation">{</span>
    index <span class="token punctuation">:</span> <span class="token punctuation">(</span>req<span class="token punctuation">,</span> res<span class="token punctuation">)</span><span class="token operator">=&gt;</span><span class="token punctuation">{</span>
        res<span class="token punctuation">.</span><span class="token function">render</span><span class="token punctuation">(</span><span class="token string">'index'</span><span class="token punctuation">)</span>
    <span class="token punctuation">}</span><span class="token punctuation">,</span>
    users <span class="token punctuation">:</span> <span class="token punctuation">(</span>req<span class="token punctuation">,</span> res<span class="token punctuation">)</span><span class="token operator">=&gt;</span><span class="token punctuation">{</span>
        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">"POSTDATA"</span><span class="token punctuation">,</span> req<span class="token punctuation">.</span>body
        <span class="token keyword">var</span> user <span class="token operator">=</span>  <span class="token keyword">new</span> <span class="token class-name">User</span><span class="token punctuation">(</span><span class="token punctuation">{</span>name<span class="token punctuation">:</span> req<span class="token punctuation">.</span>body<span class="token punctuation">.</span>name<span class="token punctuation">,</span> age<span class="token punctuation">:</span> req<span class="token punctuation">.</span>body<span class="token punctuation">.</span>age<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token comment">// Try to save that new user to the database (this is the method that actually inserts into the db) and run a callback function with an error (if any) from the operation.</span>
        user<span class="token punctuation">.</span><span class="token function">save</span><span class="token punctuation">(</span><span class="token keyword">function</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token comment">// if there is an error console.log that something went wrong!</span>
            <span class="token keyword">if</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	        console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'something went wrong'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span> <span class="token comment">// else console.log that we did well and then redirect to the root route</span>
                console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'successfully added a user!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token punctuation">}</span>
        res<span class="token punctuation">.</span><span class="token function">redirect</span><span class="token punctuation">(</span><span class="token string">'/'</span><span class="token punctuation">)</span>
        <span class="token punctuation">}</span><span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h5 id="create-routes.js">create routes.js</h5>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> controller <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">"./controller"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
module<span class="token punctuation">.</span><span class="token function-variable function">exports</span> <span class="token operator">=</span> <span class="token keyword">function</span><span class="token punctuation">(</span>app<span class="token punctuation">)</span><span class="token punctuation">{</span>
  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/'</span><span class="token punctuation">,</span> controller<span class="token punctuation">.</span>index<span class="token punctuation">)</span>
  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/cars'</span><span class="token punctuation">,</span> controller<span class="token punctuation">.</span>cars<span class="token punctuation">)</span>
  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/cars/new'</span><span class="token punctuation">,</span> controller<span class="token punctuation">.</span>newcar<span class="token punctuation">)</span>
  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/cats'</span><span class="token punctuation">,</span> controller<span class="token punctuation">.</span>cats<span class="token punctuation">)</span>
  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/cats/:catID'</span><span class="token punctuation">,</span> controller<span class="token punctuation">.</span>cat_show<span class="token punctuation">)</span>
<span class="token punctuation">}</span>
</code></pre>
<h5 id="models.js">models.js</h5>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> mongoose <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'mongoose'</span><span class="token punctuation">)</span>

mongoose<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token string">'mongodb://localhost/quotes_db'</span><span class="token punctuation">)</span>

<span class="token keyword">var</span> QuoteSchema <span class="token operator">=</span>  <span class="token keyword">new</span> <span class="token class-name">mongoose<span class="token punctuation">.</span>Schema</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
name<span class="token punctuation">:</span> String<span class="token punctuation">,</span>
quote<span class="token punctuation">:</span> String<span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">{</span>timestamps <span class="token punctuation">:</span> <span class="token boolean">true</span><span class="token punctuation">}</span><span class="token punctuation">)</span>  <span class="token comment">//https://stackoverflow.com/a/15147350/5248397</span>

module<span class="token punctuation">.</span>exports <span class="token operator">=</span> mongoose<span class="token punctuation">.</span><span class="token function">model</span><span class="token punctuation">(</span><span class="token string">'Quote'</span><span class="token punctuation">,</span> QuoteSchema<span class="token punctuation">)</span>
</code></pre>
<h4 id="mongo">Mongo</h4>
<pre class=" language-console"><code class="prism  language-console">sudo mongod
mongo
</code></pre>
<h4 id="project-tree">project tree</h4>
<p>.<br>
├── controller.js<br>
├── models.js<br>
├── routes.js<br>
├── server.js<br>
├── static<br>
│   └── css<br>
│       └── style.css<br>
└── views<br>
└── index.ejs</p>

<!--stackedit_data:
eyJoaXN0b3J5IjpbODU2NTU4OTkwXX0=
-->