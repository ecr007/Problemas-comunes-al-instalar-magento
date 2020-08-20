# Problemas-comunes-al-instalar-magento

## Si al entrar en el admin se ve la pantalla negra

<div class="post-text" itemprop="text">
<p><strong>Update</strong></p>

<p>This is Magento bug. Wrong paths to Windows are generated. The fixed fix is</p>

<p><strong>Magento 2.3.0 - 2.3.3</strong></p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="com">#/vendor/magento/framework/View/Element/Template/File/Validator.php:140</span></code></pre>

<p>the string</p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="kwd">if</span><span class="pln"> </span><span class="pun">(</span><span class="lit">0</span><span class="pln"> </span><span class="pun">===</span><span class="pln"> strpos</span><span class="pun">(</span><span class="pln">$realPath</span><span class="pun">,</span><span class="pln"> $directory</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
    </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">true</span><span class="pun">;</span><span class="pln">
</span><span class="pun">}</span></code></pre>

<blockquote>
  <p>to replace</p>
</blockquote>

<pre class="default prettyprint prettyprinted" style=""><code><span class="pln">$realDirectory </span><span class="pun">=</span><span class="pln"> $this</span><span class="pun">-&gt;</span><span class="pln">fileDriver</span><span class="pun">-&gt;</span><span class="pln">getRealPath</span><span class="pun">(</span><span class="pln">$directory</span><span class="pun">);</span><span class="pln">
</span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(</span><span class="pln">$realDirectory </span><span class="pun">&amp;&amp;</span><span class="pln"> </span><span class="lit">0</span><span class="pln"> </span><span class="pun">===</span><span class="pln"> strpos</span><span class="pun">(</span><span class="pln">$realPath</span><span class="pun">,</span><span class="pln"> $realDirectory</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
   </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">true</span><span class="pun">;</span><span class="pln">
</span><span class="pun">}</span></code></pre>

<p><strong>Magento 2.2.7</strong></p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="str">/vendor/</span><span class="pln">magento</span><span class="pun">/</span><span class="pln">framework</span><span class="pun">/</span><span class="typ">View</span><span class="pun">/</span><span class="typ">Element</span><span class="pun">/</span><span class="typ">Template</span><span class="pun">/</span><span class="typ">File</span><span class="pun">/</span><span class="typ">Validator</span><span class="pun">.</span><span class="pln">php</span><span class="pun">:</span><span class="lit">113</span></code></pre>

<p>code</p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="kwd">protected</span><span class="pln"> </span><span class="kwd">function</span><span class="pln"> isPathInDirectories</span><span class="pun">(</span><span class="pln">$path</span><span class="pun">,</span><span class="pln"> $directories</span><span class="pun">)</span><span class="pln">
</span><span class="pun">{</span><span class="pln">
    </span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(!</span><span class="pln">is_array</span><span class="pun">(</span><span class="pln">$directories</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
        $directories </span><span class="pun">=</span><span class="pln"> </span><span class="pun">(</span><span class="pln">array</span><span class="pun">)</span><span class="pln">$directories</span><span class="pun">;</span><span class="pln">
    </span><span class="pun">}</span><span class="pln">
    </span><span class="kwd">foreach</span><span class="pln"> </span><span class="pun">(</span><span class="pln">$directories </span><span class="kwd">as</span><span class="pln"> $directory</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
        </span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(</span><span class="lit">0</span><span class="pln"> </span><span class="pun">===</span><span class="pln"> strpos</span><span class="pun">(</span><span class="pln">$this</span><span class="pun">-&gt;</span><span class="pln">fileDriver</span><span class="pun">-&gt;</span><span class="pln">getRealPath</span><span class="pun">(</span><span class="pln">$path</span><span class="pun">),</span><span class="pln"> $directory</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
            </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">true</span><span class="pun">;</span><span class="pln">
        </span><span class="pun">}</span><span class="pln">
    </span><span class="pun">}</span><span class="pln">
    </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">false</span><span class="pun">;</span><span class="pln">
</span><span class="pun">}</span></code></pre>

<p>to replace</p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="kwd">protected</span><span class="pln"> </span><span class="kwd">function</span><span class="pln"> isPathInDirectories</span><span class="pun">(</span><span class="pln">$path</span><span class="pun">,</span><span class="pln"> $directories</span><span class="pun">)</span><span class="pln">
    </span><span class="pun">{</span><span class="pln">
        $realPath </span><span class="pun">=</span><span class="pln"> str_replace</span><span class="pun">(</span><span class="str">'\\'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'/'</span><span class="pun">,</span><span class="pln"> $this</span><span class="pun">-&gt;</span><span class="pln">fileDriver</span><span class="pun">-&gt;</span><span class="pln">getRealPath</span><span class="pun">(</span><span class="pln">$path</span><span class="pun">));</span><span class="pln">
        </span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(!</span><span class="pln">is_array</span><span class="pun">(</span><span class="pln">$directories</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
            $directories </span><span class="pun">=</span><span class="pln"> </span><span class="pun">(</span><span class="pln">array</span><span class="pun">)</span><span class="pln">$directories</span><span class="pun">;</span><span class="pln">
        </span><span class="pun">}</span><span class="pln">
        </span><span class="kwd">foreach</span><span class="pln"> </span><span class="pun">(</span><span class="pln">$directories </span><span class="kwd">as</span><span class="pln"> $directory</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
            </span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(</span><span class="lit">0</span><span class="pln"> </span><span class="pun">===</span><span class="pln"> strpos</span><span class="pun">(</span><span class="pln">$realPath</span><span class="pun">,</span><span class="pln"> $directory</span><span class="pun">))</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
                </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">true</span><span class="pun">;</span><span class="pln">
            </span><span class="pun">}</span><span class="pln">
        </span><span class="pun">}</span><span class="pln">
        </span><span class="kwd">return</span><span class="pln"> </span><span class="kwd">false</span><span class="pun">;</span><span class="pln">
    </span><span class="pun">}</span></code></pre>

<p>If You can't find out the (/vendor/magento/framework/) folder in magento 2.2.7 - 2.3.3 . Then You can check it here:</p>

<pre class="default prettyprint prettyprinted" style=""><code><span class="com">#lib\internal\Magento\Framework\View\Element\Template\File\Validator.php</span></code></pre>
    </div>
    
## Si no cargan los assents en el frontent

clear files in var/cache/TODO, var/page_cache/TODO
