<h1>Table of Contents<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#��-docstring-��ʼ" data-toc-modified-id="��-docstring-��ʼ-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>�� docstring ��ʼ</a></span></li><li><span><a href="#����-@doc-����ע��" data-toc-modified-id="����-@doc-����ע��-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>���� @doc ����ע��</a></span></li><li><span><a href="#ʵ��-MD-ʵ��:-���ַ�����-MD-����" data-toc-modified-id="ʵ��-MD-ʵ��:-���ַ�����-MD-����-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>ʵ�� MD ʵ��: ���ַ����� MD ����</a></span><ul class="toc-item"><li><span><a href="#�ú�ͺ�������-markdown-�ַ���" data-toc-modified-id="�ú�ͺ�������-markdown-�ַ���-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>�ú�ͺ������� markdown �ַ���</a></span></li><li><span><a href="#����ʽ����齨-markdown-�ṹ" data-toc-modified-id="����ʽ����齨-markdown-�ṹ-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>����ʽ����齨 markdown �ṹ</a></span></li></ul></li><li><span><a href="#MD-����ת����-String" data-toc-modified-id="MD-����ת����-String-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>MD ����ת���� String</a></span></li><li><span><a href="#ʵ���ĵ�����ת��:-md-�ļ�-ת����-html-�ļ���-tex-�ļ�" data-toc-modified-id="ʵ���ĵ�����ת��:-md-�ļ�-ת����-html-�ļ���-tex-�ļ�-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>ʵ���ĵ�����ת��: md �ļ� ת���� html �ļ��� tex �ļ�</a></span></li></ul></div>


Julia �ı�׼�� `Markdown.jl` ������Ч�ؽ��� markdown, �� Julia 0.4 �汾��ʼ, `Markdown.jl` ���Ϊ Julia �Ļ�����. 

�����Ǵ� REPL �������ģʽ(�� `?` ��), ������ Jupyter Notebook ��ʹ�� `?` �鿴�����İ�����Ϣʱ, �ḻ��ʵİ������ֱ����� `Markdown.jl` �ڷ�������.

---



# �� docstring ��ʼ

�鿴 `+` ��˵���ĵ�ʱ, ��ᷢ�����塢��ɫ����ʽ������Ψһ�ģ��������ʵ�ֵ���

![REPL_docstring](https://raw.githubusercontent.com/ZIP97/JuliaRepo/master/markdown.jlʹ���ܽ�/REPL_docstring.png)

![JN_docstring](https://raw.githubusercontent.com/ZIP97/JuliaRepo/master/markdown.jlʹ���ܽ�/JN_docstring.png)

>  ҪŪ�����һ��, ������Ҫ���뼸�� macro �� `MD` �������


```julia
using Markdown
using Markdown: @doc, @doc_str, MD
```

> ���Ƚ� `+` �� docstr ��������, ʹ�� @doc ���浽 *doc* ����, ���Է���, *doc* �������� **MD**, �������;��� markdown �� Julia �еı������Ķ���, �� **MD** ��������, һ������ *meta*, ��ӳ������Ԫ��Ϣ, ��һ������ *content* ��һ������Ϊ **MD** ��һά array, ��������˵���������÷�. Ҳ����˵, docstring �����ɸ��� **MD** ʵ�����.


```julia
doc = @doc +; # type: MD
doc.meta
```


    Dict{Any,Any} with 3 entries:
      :typesig => Union{}
      :binding => Base.:+
      :results => Base.Docs.DocStr[DocStr(svec("    +(x, y...)\n\nAddition operator��




```julia
doc.content
```


    2-element Array{Any,1}:
     ```
    +(x, y...)
    ```
    
    Addition operator. `x+y+z+...` calls this function with all arguments, i.e. `+(x, y, z, ...)`.
    
    # Examples
    
    ```jldoctest
    julia> 1 + 20 + 4
    25
    
    julia> +(1, 20, 4)
    25
    ```



> ��ô����뵽, ����������� `MD` ����ʵ��, ���������ϳ������� docstring ��?
>
> `MD` �������԰�����ʵ��. �������������ͷ���
>
> - ��������Ϊ **MD** ��һά array ʱ, ����������γ�һ���µ� **MD**
>
> - ��������Ϊ **Dict** ����ʱ, ����ӵ� **MD** ʵ���� meta ����, ��Ϊ����˵��
>
> �������˼·, �����д�� `+` �� *α* docstr

```julia
docPseudo = MD(Vector{MD}(
        [doc.content[1], doc.content[2]]
        )
);
docPseudo.meta = Dict(:typesig => Union{}, :binding => Base.:+, :results => docPseudo);
docPseudo # ����� + ���ĵ���һ����, ����ƪ���Ͳ���������
```

---



# ���� @doc ����ע��

> �������������, ����֪�� �ĵ��ַ����� Julia �о��� MD ����, ��ô��ν��ַ���"���"�������ĵ���?
>
> ��: ʹ�� `@doc` ��
>
> �������� `@doc @doc` ���� `?@doc`, ��ᷢ�� `@doc` ����������:
>
> 1. ��ȡĳ�����󣨺������ꡢ���������ĵ�
> 2. ��ĳ�����󣨺������ꡢ����������ĵ�
>
> ������Ѿ�֪������ Julia �������ں�����ǰ�� `"""` ��������ע�ͣ�

    """
    print `param` (take **Int** type as the argument)
    
    # First Examples
    
    ```julia
    julia> str = "Hello World\n"
    julia> Print(str*str)
    $(let str = "Hello World"
        str*str
    end)
    ```
    """
    function Print(param::String)
        println(param)
        end

> ���� REPL ��, ���Ϸ�ʽ���޷����ַ����������ĵ��ַ���(���н��ֻ�ǵ�����һ���ַ�����һ��������), ʹ�� `md` `doc` �ַ������������ע��ʱ�ᱨ `ERROR: syntax` �﷨����
>
> ��ʱӦʹ�� `@doc` ��� `->` ���Ÿ�����(�����κ�����Ҫע�͵Ķ���)��ע��

    @doc """
    print `param` (take **Float64** type as the argument)
    
    # Second Examples
    
    ```julia
    julia> r = rand()
    julia> Print(r)
    $(let r=rand()
        r
    end)
    ```
    """ ->
    function Print(param::Float64)
        println(param)
        end

>  ����

    docstring = """
    print `param` (take **String** type as the argument)
    
    # Third Examples
    
    ```julia
    julia> Print(1)
    1
    ```
    """;
    
    @doc docstring Print(param::Int) = print(param)

> �鿴 `Print` ���ĵ�: 

```julia
?Print
```


print `param` (take **Int** type as the argument)

# First Examples

```julia
julia> str = "Hello World
"
julia> Print(str*str)
Hello WorldHello World
```

---

print `param` (take **Float64** type as the argument)

# Second Examples

```julia
julia> r = rand()
julia> Print(r)
0.40218381068451414
```

---

print `param` (take **String** type as the argument)

# Third Examples

```julia
julia> Print(1)
1
```

> ��ᾪϲ�ط���, ���� method ������, docstring Ҳ�ܹ���������, ���������Ҫ����ĳ�� method, ֻ��Ҫ����ʹ�� `@doc` ����ע��


```julia
@doc "# Same Type as String" Print(param::String) = print(param);
?Print # ���� @doc Print
```

# Same Type as String

---

print `param` (take **Float64** type as the argument)

# Second Examples
......(���������ʡ��)

> ���˺���, Julia �����ԶԳ�������������������������͵Ķ������ע��


```julia
# �� MD �������ע��
mdObject = doc"mdObject"
@doc "this belongs to `md` object" mdObject
@doc mdObject
# ���Ϊ: this belongs to `md` object
```


```julia
# �Գ���ע��
@doc "# Constant
**$(typeof(imag))** type

the square is $(imag*imag)" -> 
const imag = 1im

@doc imag
```




# Constant

**Complex{Int64}** type

the square is -1 + 0im


> ������������ӿ��Կ���, Julia �Ĳ�ֵ���ܷǳ�ǿ��, Julia �ܹ��Ƚ�������ֵ"����"����, Ȼ��"����"�� docstring. Ȼ�� docstring �����ܹ���̬�ذ�ֵ"����"����, �뿴��������:


```julia
@doc "# Vector of Random Variables
param `n`: length of the vector
defalut `n` = $(length(num))
"  num = rand(3);
@doc num
```


# Vector of Random Variables

param `n`: length of the vector

defalut `n` = 3



```julia
num= rand(1);
@doc num
```


# Vector of Random Variables

param `n`: length of the vector

defalut `n` = 3



>������ **num** ��ֵ, ���� docstring ��Ȼ�ǵ�һ�θ�ֵ�Ľ��, �ںܶೡ������ǳ�ʵ��, �����ǶԳ���ע��
>
>�������ǿ�����ѭ֮ǰ���۵�˼·, ���µ� MD ������и���

```julia
@doc "# Vector of Random Variables
param `n`: length of the vector

defalut `n` = $(length(num))
"  num = rand(100)

@doc num
```


# Vector of Random Variables

param `n`: length of the vector

defalut `n` = 100

---



# ʵ�� MD ʵ��: ���ַ����� MD ����



>������, �����֪��, ���д�� **MD** ʵ��, �����ַ�ʽ:
>
>1. �Է��� markdown �﷨���ַ�����ʹ�� `md` �� `doc` �ַ����� (string macro)��`@doc_str` ��(macro) ��`Markdown.parse()`����ֱ��ת���� MD ����
>2. ����ͨ�ַ�����ʹ�� `Markdown.jl`����غ���: Header�� LaTeX�� Image�� Link�� Bold�� Italic�� Table�� Code�� Footnote�� Paragraph �Ƚ�����Ӧ���齨 markdown �ṹ



## �ú�ͺ������� markdown �ַ���


```julia
md_logo = md"""
# Julia Icon

![Julia Icon](https://raw.githubusercontent.com/JuliaLang/julia-logo-graphics/master/images/old-style/julia-logo-488-by-338.png)
""";


doc_str_logo = @doc_str """
# Julia Dragon Icon
![Julia Dragon Icon](https://discourse.juliacn.com/uploads/default/original/1X/6422ed319b31937c1e13c9dc7ab6b5166db5ba79.jpeg)
""";

str = """
# Julia Three-Balls Icon
![Julia Three-Balls Icon](https://raw.githubusercontent.com/JuliaLang/julia-logo-graphics/master/images/old-style/three-balls.png)
"""
parse_logo = Markdown.parse(str);

mdChunk = MD(Vector{MD}(
        [doc_str_logo, md_logo, parse_logo]
        )
)
```

# Julia Dragon Icon

![Julia Dragon Icon](https://discourse.juliacn.com/uploads/default/original/1X/6422ed319b31937c1e13c9dc7ab6b5166db5ba79.jpeg)

# Julia Icon

![Julia Icon](https://raw.githubusercontent.com/JuliaLang/julia-logo-graphics/master/images/old-style/julia-logo-488-by-338.png)

# Julia Three-Balls Icon

![Julia Three-Balls Icon](https://raw.githubusercontent.com/JuliaLang/julia-logo-graphics/master/images/old-style/three-balls.png)


> �ڽ������в�ֵ�ַ���ʱ, �����ַ�ʽ���в�ͬ, `md` �ַ������Ѳ���Բ�ֵ������ֵ, `parse` ������Բ�ֵ��ֵ, �� `@doc_str` ���޷��������в�ֵ���ַ���


    md"""
    print `str` (only take **String** type as the argument)
    
    # Examples
    
    ```julia
    julia> Print("Hello World")
    $(let str="Hello World"
       str
      end
    )
    ```
    """

print `str` (only take **String** type as the argument)

# Examples

```julia
julia> Print("Hello World")
$(let str="Hello World"
   str
  end
)
```

<br>


    Markdown.parse("""
    print `str` (only take **String** type as the argument)
    
    # Examples
    
    ```julia
    julia> Print("Hello World")
    $(let str="Hello World"
       str
      end
    )
    ```
    """)

print `str` (only take **String** type as the argument)

# Examples

```julia
julia> Print("Hello World")
Hello World
```

<br>


    @doc_str """
    print `str` (only take **String** type as the argument)
    
    # Examples
    
    ```julia
    julia> Print("Hello World")
    $(let str="Hello World"
       str
      end
    )
    ```
    """

<br>

    MethodError: no method matching @doc_str(::LineNumberNode, ::Module, ::Expr)
    Closest candidates are:
      @doc_str(::LineNumberNode, ::Module, !Matched::AbstractString, !Matched::Any...) at /buildworker/worker/package_linux64/build/usr/share/julia/stdlib/v1.3/Markdown/src/Markdown.jl:56

---



## ����ʽ����齨 markdown �ṹ


```julia
using Markdown
using Markdown: MD,  Header, Italic, Bold,  plain, Code, LaTeX, Paragraph, html
```

> ���� markdown �﷨�� HTML �ṹ, ����֪�� md �е� `# title` д����ʵ�Ƕ�Ӧ html �� `<h1>title<\h1>`
> �� Julia ��, �ú����ķ�ʽ�� **h1** ������Ӧ MD ����Ĺ�����: ��ʹ�� `Header()` ���� **Header** ����, ���� `MD()` ��������һά����, ��������������ת����һ������� MD ����:



```julia
h1 = Header("title") # type => Header{1}
md_header = MD(h1) # type => MD
md_header == md"# title" # => ���: true
```


> �ٿ�һ���򵥵� md ��� md1


```julia
 md1 = md"foo *italic foo* **bold foo** `code foo`"
```

foo *italic foo* **bold foo** `code foo`


> ���������Ҫʵ�� md1 �� markdown �ṹ, �ú�������ʽ, ��Ҫд��


```julia
md2 = MD(Paragraph(["foo ", Italic("italic foo"), " ", Bold("bold foo"), " ",  Code("code foo")]))
md1 == md2 # => ���: true
```

> ͬ����, ���ǿ���"�ϲ�"��ͬ���͵�������Ϊһ������� `MD` ����:


```julia
md3 = md"""inline $\LaTeX$ formula 

$\frac{1}{2}$

interline formula $$\frac{1}{2}$$
""";

LATEX = LaTeX("\\text{end with}\\, \\LaTeX \\cdots");
v = [Header("MD ʵ��"), md3, 
    Header("MD ʵ��ת������ͨ�ַ���"), plain(md3),
    Header("LaTeX ��ʽ"), LATEX]
md_v = MD(v);

map(x->typeof(x), vcat(v, md_v))
```

    7-element Array{DataType,1}:
     Header{1}
     MD       
     Header{1}
     String   
     Header{1}
     LaTeX    
     MD       



> `@doc`�ĵ�һ����������֧���ַ���, ��֧�� MD ����:


```julia
o5 = .5;
@doc md_v o5;
@doc o5
```

# MD ʵ��

inline $\LaTeX$ formula 

$$
\frac{1}{2}
$$

interline formula $\frac{1}{2}$

# MD ʵ��ת������ͨ�ַ���

"inline \$\\LaTeX\$ formula \n\n\$\$\n\\frac{1}{2}\n\$\$\n\ninterline formula \$\\frac{1}{2}\$\n"

# LaTeX ��ʽ

$$
\text{end with}\, \LaTeX \cdots
$$

---



# MD ����ת���� String

> MD �������ת�������������ַ���:
> - md ��ʽ���ַ���: `plain()` ����
> - html ��ʽ���ַ���: `html()` ����
> - tex ��ʽ���ַ���: `latex()` ����



```julia
 md1 = md"foo *italic foo* **bold foo** `code foo`"
```

foo *italic foo* **bold foo** `code foo`


```julia
plain(md1)
```

    "foo *italic foo* **bold foo** `code foo`\n"


```julia
html(md1)
```

    "<p>foo <em>italic foo</em> <strong>bold foo</strong> <code>code foo</code></p>\n"




```julia
latex(md1)
```

    "foo \\emph{italic foo} \\textbf{bold foo} \\texttt{code foo}\n\n"

---



# ʵ���ĵ�����ת��: md �ļ� ת���� html �ļ��� tex �ļ�



> MD ���󲻽�������������ĵ�˵��, ������ʵ�� markdown �������ĵ������໥ת��
> ����������һ�� markdown �ļ� (Ϊ����˵��,  ���Ǵ� github ������ Markdown.jl �� md �ĵ�)



```julia
using HTTP

url = "https://raw.githubusercontent.com/JuliaStdlibs/Markdown.jl/master/docs/src/index.md"
r = HTTP.request("GET", url; verbose=1); 
# verbose �������ڴ�ӡ����Ŀͻ��˺ͷ�����֮����Ϣ��, 0 ��ʾ����ʾ��Ϣ, 1 ��ʾ��ӡ��򵥵Ľ�����Ϣ, 2 ��ʾ��ӡ request ����� response ��Ӧ������Ϣ, 3 ��ʾ��ӡ��ϸ�� Debug ��Ϣ

mdString = String(r.body); # type: str
open(f -> write(f, mdString), "index.md", "w"); # �������Ϊ index.md �ı����ļ�
```

```julia
# ������ md �ļ������� MD ����
mdInstance = Markdown.parse_file(joinpath(pwd(), "index.md")); # => type: MD
```

> ������Ȱ� MD ����ת���� String, Ȼ��� String ������ı��ļ�:


```julia
mdHtml = html(mdInstance); # => type: str 
open(f -> write(f, mdHtml), "index.html", "w");
```


```julia
mdLatex = latex(mdInstance); # => type: str 
open(f -> write(f, mdLatex), "index.tex", "w");
```

������һ�ּ���ת��������ʹ�� `html(io::IO, md::MD)` `latex(io::IO, md::MD)` ����, ֱ�ӽ��� IO ���� (������� [multiple dispatch](https://docs.julialang.org/en/v1/manual/methods/index.html))


```julia
open(f->Markdown.html(f, mdInstance), "readme.html", "w")
# �ȼ���
# io = open("readme.html", "w");
# Markdown.html(io, mdInstance);
# close(io)
# ����ʹ�� tohtml ����
# open(f->Markdown.tohtml(f, mdInstance), "readme.html", "w")
```

![ת��html](https://raw.githubusercontent.com/ZIP97/JuliaRepo/master/markdown.jlʹ���ܽ�/ת��html.png)


```julia
open(f->Markdown.latex(f, mdInstance), "readme.tex", "w")
```

![ת��tex](https://raw.githubusercontent.com/ZIP97/JuliaRepo/master/markdown.jlʹ���ܽ�/ת��tex.png)



> �ο�����
>
> 1. Markdoan.jl github��ַ: https://github.com/JuliaStdlibs/Markdown.jl
> 2. Julia �ٷ��ĵ�����: https://docs.julialang.org/en/latest/stdlib/Markdown/
> 3. ���ⲩ�� *MonthOfJulia Day 36: Markdown*: https://www.juliabloggers.com/monthofjulia-day-36-markdown/
> 4. markdown �﷨���Ľ���: http://www.markdown.cn/
> 5. markdown �﷨ cheatsheet: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet



<div style="color:#7a7777; font-style:italic; background: #fffac6; border-radius: 25px; padding: 20px; box-shadow: 10px 10px 5px grey;">���� jupyter �汾 : <a href="https://github.com/ZIP97/JuliaRepo/blob/master/markdown.jlʹ���ܽ�/Markdown.jl ʹ���ܽ�.ipynb">https://github.com/ZIP97/JuliaRepo/blob/master/markdown.jlʹ���ܽ�/Markdown.jl ʹ���ܽ�.ipynb</div>

