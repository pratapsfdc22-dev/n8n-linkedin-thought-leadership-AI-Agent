TASK
Rewrite ONLY the failing “Sources” bullets below. Return the SAME number of bullets in the SAME order, using public, direct **HTTPS** URLs.

STRICT RULES
- No redirects or link shorteners. No tracking params. No paywalls.
- Ban: lnkd.in, bit.ly, t.co, tinyurl.com, ow.ly, rebrand.ly, buff.ly, goo.gl, shorturl.at, cutt.ly.
- Prefer reputable, public domains (examples): arxiv.org • nist.gov • mit.edu • stanford.edu • pewresearch.org • openai.com/research • anthropic.com/news • deepmind.google/discover • microsoft.com/research • cloud.google.com/blog • aws.amazon.com/blogs • meta.com/research • salesforce.com/blog • zendesk.com/blog or /cx/trends • intercom.com/blog • hubspot.com/research • ibm.com/blog • adobe.com/insights • thinkwithgoogle.com • iab.com.
- If uncertain a URL exists publicly, choose a different credible source you are confident about.

FAILING BULLETS (in order)
<<<
{{$json.bad_bullets.join("\n")}}
>>>

RETURN FORMAT (exactly these lines, no extra text)
- <Title or short description> — <direct HTTPS URL>
- <Title or short description> — <direct HTTPS URL>
- <repeat for each failing bullet, preserving order>
