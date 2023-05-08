Download Link: https://assignmentchef.com/product/solved-sta303-assignment-3
<br>









<strong>1 Qi Birth</strong>

The story in the Globe and Mail titled “Fewer boys born in Ontario after Trump’s 2016 election win, study finds” at <a href="https://www.theglobeandmail.com/canada/article-fewer-boys-born-in-ontario-after-trumps-2016-election-win-study">www.theglobeandmail.com/canada/article-fewer-boys-born-in-ontario-after-trumps-2016-election­</a><u>win-study</u> refers to the paper by Retnakaran and Ye (2020). The hypothesis being investigated is that following the election of Donald Trump the proportion of babies born who are male fell. Women in the early stages of pregnancy are susceptible to miscarriage or spontaneous abortion when put under stress, and for biological reasons male fetuses are more at risk than female fetuses. Retnakaran and Ye (2020) use birth data from Ontario, and found the reduction in male babies was more pronounced in liberal-voting areas of the province than conservative-voting areas. Births in March 2017, which would have been 3 or 4 months gestation at the time of the November 2016 election, are shown to be particularly affected by the results of the election.

For testing the hypothesis that stress induced by Trump’s election is affecting the sex ratio at birth, the choice of Ontario as the study population by Retnakaran and Ye (2020) is an odd one. The dataset below considers was retrieved from <a href="http://wonder.cdc.gov">wonder.cdc.gov</a>, and contains monthly birth counts in the US for Hispanics and Non-Hispanic Whites, for rural and urban areas. Rural whites voted for Trump in large numbers, and would presumably not be stressed by the results of the election. Urban areas voted against Trump for the most part, and Americans of Hispanic origin had many reasons to be anxious following Trump’s election. 1 shows birth numbers and ratio of male to female births for rural Whites and urban Hispanics over time.

theFile = ‘birthData.rds’

<strong>if</strong>(!<strong>file.exists</strong>(theFile)) {

<strong>download.file</strong>(‘<a href="http://pbrown.ca/teaching/303/data/birthData.rds',">http://pbrown.ca/teaching/303/data/birthData.rds’,</a> theFile)

}

x =<strong> readBDS</strong>(theFile)

A Generalized Additive model was fit to these data by first defining some variables, and creating a cbygroup’ variable that’s a unique urban/hispanic indicator.

x$bygroup =<strong> factor</strong>(<strong>gsub</strong>(“[[:space:]]”, “”,<strong> pasteO</strong>(x$MetroNonmetro, x$MothersHispanicOrigin))) x$timelnt =<strong> as.numeric</strong>(x$time)

x$y =<strong> as.matrix</strong>(x[,<strong>c</strong>(‘Male’,’Female’)]) x$sin12 =<strong> sin</strong>(x$timelnt/365.25)

x$cos12 =<strong> cos</strong>(x$timelnt/365.25) x$sin6 =<strong> sin</strong>(2*x$timelnt/365.25) x$cos6 =<strong> cos</strong>(2*x$timelnt/365.25) baselineDate =<strong> as.Date</strong>(‘2007/1/1’)

baselineDatelnt =<strong> as.integer</strong>(baselineDate)

The GAM model was fit as follows.

res = mgcv::<strong>gam</strong>(y – bygroup + cos12 + sin12 +<sub> cos6</sub> + sin6





<strong>s</strong>(timelnt, by=bygroup, k = 120, pc=baselineDatelnt), data=x, family=<strong>binomial</strong>(link=’logit’))

A Generalized Linear Mixed Model was fit below.

res2 = gamm4::<strong>gamm4</strong>(y – bygroup + cos12 + sin12 +<sub> cos6</sub> + sin6 + <strong>s</strong>(timelnt, by=bygroup, k = 120, pc=baselineDatelnt),

random = -(1lbygroup:timelnt),

data=x, family=<strong>binomial</strong>(link=’logit’))

coefGamm =<strong> summary</strong>(res2$mer)$coef

knitr::<strong>kable</strong>(<strong>cbind</strong>(

mgcv::<strong>summary.gam</strong>(res)$p.table[,1:2],

<table>

 <tbody>

  <tr>

   <td colspan="3" width="403">coefGamm[<strong>grep</strong>(“~Xs[(]”,<strong> rownames</strong>(coefGamm), invert=TRUE), digits=5)</td>

   <td width="72">1:2]),</td>

   <td width="70"> </td>

  </tr>

  <tr>

   <td width="262"> </td>

   <td width="68">Estimate</td>

   <td width="73">Std. Error</td>

   <td width="72">Estimate</td>

   <td width="70">Std. Error</td>

  </tr>

  <tr>

   <td width="262">(Intercept)</td>

   <td width="68">0.03237</td>

   <td width="73">0.00583</td>

   <td width="72">0.04223</td>

   <td width="70">0.00128</td>

  </tr>

  <tr>

   <td width="262">bygroupMetroNotllispanicorLatino</td>

   <td width="68">0.01942</td>

   <td width="73">0.00640</td>

   <td width="72">0.00678</td>

   <td width="70">0.00149</td>

  </tr>

  <tr>

   <td width="262">bygroupNonmetrollispanicorLatino</td>

   <td width="68">-0.02340</td>

   <td width="73">0.02013</td>

   <td width="72">-0.00643</td>

   <td width="70">0.00455</td>

  </tr>

  <tr>

   <td width="262">bygroupNonmetroNotllispanicorLatino</td>

   <td width="68">0.01550</td>

   <td width="73">0.00604</td>

   <td width="72">0.00593</td>

   <td width="70">0.00209</td>

  </tr>

  <tr>

   <td width="262">cos12</td>

   <td width="68">0.00060</td>

   <td width="73">0.00125</td>

   <td width="72">-0.00026</td>

   <td width="70">0.00048</td>

  </tr>

  <tr>

   <td width="262">sin12</td>

   <td width="68">-0.00021</td>

   <td width="73">0.00123</td>

   <td width="72">0.00046</td>

   <td width="70">0.00047</td>

  </tr>

  <tr>

   <td width="262">cos6</td>

   <td width="68">0.00165</td>

   <td width="73">0.00116</td>

   <td width="72">0.00092</td>

   <td width="70">0.00045</td>

  </tr>

  <tr>

   <td width="262">sin6</td>

   <td width="68">0.00071</td>

   <td width="73">0.00118</td>

   <td width="72">0.00010</td>

   <td width="70">0.00046</td>

  </tr>

 </tbody>

</table>




1/<strong>sqrt</strong>(res$sp)

##                        s(timelnt):bygroupMetroHispanicorLatino

##                                                                                                                          5.201104e-01

##            s(timelnt):bygroupMetroNotHispanicorLatino

##                                                                                                                          2.224808e-01

##              s(timelnt):bygroupNonmetroHispanicorLatino

##                                                                                                                             1.284191e+00

## s(timelnt):bygroupNonmetroNotHispanicorLatino##                                                                                                                          2.847145e-05

lme4::<strong>VarCorr</strong>(res2$mer)

<table>

 <tbody>

  <tr>

   <td width="25">##</td>

   <td width="115">Groups</td>

   <td width="321">Name</td>

   <td width="187">Std.Dev.</td>

  </tr>

  <tr>

   <td width="25">##</td>

   <td width="115">bygroup:timelnt</td>

   <td width="321">(lntercept)</td>

   <td width="187">0.0022596</td>

  </tr>

  <tr>

   <td width="25">##</td>

   <td width="115">Xr.2</td>

   <td width="321">s(timelnt):bygroupNonmetroNotHispanicorLatino</td>

   <td width="187">0.0000000</td>

  </tr>

  <tr>

   <td width="25">##</td>

   <td width="115">Xr.1</td>

   <td width="321">s(timelnt):bygroupNonmetroHispanicorLatino</td>

   <td width="187">0.0000000</td>

  </tr>

  <tr>

   <td width="25">##</td>

   <td width="115">Xr.0</td>

   <td width="321">s(timelnt):bygroupMetroNotHispanicorLatino</td>

   <td width="187">0.0000000</td>

  </tr>

  <tr>

   <td width="25">##</td>

   <td width="115">Xr</td>

   <td width="321">s(timelnt):bygroupMetroHispanicorLatino</td>

   <td width="187">0.0000000</td>

  </tr>

 </tbody>

</table>




Predict seasonally adjusted time trend (birth ratio assuming every month is January)

timeJan =<strong> as.numeric</strong>(<strong>as.Date</strong>(‘2010/1/1’))/365.25

toPredict =<strong> expand.grid</strong>(

timelnt =<strong> as.numeric</strong>(<strong>seq</strong>(<strong>as.Date</strong>(‘2007/1/1’),<strong> as.Date</strong>(‘2018/12/1′), by=’1 day’)), bygroup =<strong> c</strong>(‘MetroHispanicorLatino’, ‘NonmetroNotHispanicorLatino’),

cos12 =<strong> cos</strong>(timeJan), sin12 =<strong> sin</strong>(timeJan), cos6 =<strong> cos</strong>(timeJan/2), sin6 =<strong> sin</strong>(timeJan/2) )

predictGam = mgcv::<strong>predict</strong><strong>.gam</strong>(res, toPredict, se.fit=TRUE)

predictGamm =<strong> predict</strong>(res2$gam, toPredict, se.fit=TRUE)




these are shown in 2.




<table>

 <tbody>

  <tr>

   <td width="268">

    <table width="100%">

     <tbody>

      <tr>

       <td> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="268">

    <table width="100%">

     <tbody>

      <tr>

       <td> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="234">

    <table width="100%">

     <tbody>

      <tr>

       <td>2008 2010 2012 2014 2016 2018</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="267">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="160">

    <table width="100%">

     <tbody>

      <tr>

       <td>MetroHispanicorLatino NonmetroNotHispanicorLatino</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="234">

    <table width="100%">

     <tbody>

      <tr>

       <td>2008 2010 2012 2014 2016 2018</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="267">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="160">

    <table width="100%">

     <tbody>

      <tr>

       <td>MetroHispanicorLatino NonmetroNotHispanicorLatino</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




time

(a) gam time

(b) gamm




Figure 2: Predicted time trends

Predict independent random effects

ranef2 = 1me4::<strong>ranef</strong>(res2$mer, condVar=TRUE, whichel = ‘bygroup:timelnt’)

ranef2a =<strong> exp</strong>(<strong>cbind</strong>(est=ranef2[[1]][[1]], se=<strong>sqrt</strong>(<strong>attributes</strong>(ranef2[[1]])$postVar)) °h *°h theCiMat)




<table>

 <tbody>

  <tr>

   <td width="305">

    <table width="100%">

     <tbody>

      <tr>

       <td> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="305">

    <table width="100%">

     <tbody>

      <tr>

       <td> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="305">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="126">

    <table width="100%">

     <tbody>

      <tr>

       <td>These are shown in 3</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="224">

    <table width="100%">

     <tbody>

      <tr>

       <td>2014                          2016                           2018</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="302">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="225">

    <table width="100%">

     <tbody>

      <tr>

       <td>2014                           2016                          2018</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




time

(a) MetroHispanicorLatino time

(b) NonmetroNotHispanicorLatino




Figure 3: bygroup:timelnt random effects

<ol>

 <li>Write down statistical models corresponding to res and res2</li>

 <li>Which of the two sets of results is more useful for investigating this research hypothesis?</li>

 <li>Write a short report (a paragraph or two) addressing the following hypothesis: The long-term trend in sex ratios for urban Hispanics and rural Whites is consistent with the hypothesis that discrimination against Hispanics, while present in the full range of the dataset, has been increasing in severity over</li>

 <li>Write a short report addressing the following hypothesis: The election of Trump in November 2016 had a noticeable effect on the sex ratio of Hispanic-Americans roughly 5 months after the election.</li>

</ol>




<strong>2 Q2 Death</strong>

This is the same data as you saw in the lab, but has been updated to 23 March.

<strong>if</strong>(!<strong>requireNamespace</strong>(“nCov2019”)) { devtools::<strong>install_github</strong>(“GuangchuangYu/nCov2019”)

}

x1 &lt;- nCov2019::<strong>load_nCov2O19</strong>(lang = ‘en’)

hubeI = x1$provInce[<strong>which</strong>(x1$provInce$provInce == ‘HubeI’), ]

hubeI$deaths =<strong> c</strong>(0,<strong> diff</strong>(hubeI$cum_dead))

Italy = x1$global[<strong>which</strong>(x1$global$country == ‘Italy’), ]

Italy$deaths =<strong> c</strong>(0,<strong> diff</strong>(Italy$cum_dead)) x =<strong> list</strong>(HubeI= hubeI, Italy=Italy)

<strong>for</strong>(D<strong> in names</strong>(x)) {

<strong>plot</strong>(x[[D]][,<strong>c</strong>(‘tIme’,’deaths’)], xlIm =<strong> as.Date</strong>(<strong>c</strong>(‘2020/1/10’, ‘2020/4/1’)))

}




<table>

 <tbody>

  <tr>

   <td width="619">

    <table width="100%">

     <tbody>

      <tr>

       <td> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="76">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="76">

    <table width="100%">

     <tbody>

      <tr>

       <td><strong>● </strong>●●●●●●●●<strong>●</strong>●●<strong>●●</strong>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="19">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="41">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●● ●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="251">

    <table width="100%">

     <tbody>

      <tr>

       <td>● ● ●●●<strong>●</strong>●● <strong>● </strong>●●●●●●●●●●●●●●●●●●<strong>●●●</strong>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="110">

    <table width="100%">

     <tbody>

      <tr>

       <td>

        <table>

         <tbody>

          <tr>

           <td width="54">●●●●●<strong>●</strong></td>

           <td width="52">●● ●●●●</td>

          </tr>

         </tbody>

        </table> </td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="26">

    <table width="100%">

     <tbody>

      <tr>

       <td>● ●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="37">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="102">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="10">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="19">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="13">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="16">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="13">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="16">

    <table width="100%">

     <tbody>

      <tr>

       <td><strong>● ●</strong></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




Feb 01                                                          Mar 01                            Apr 0                                                                                                 Feb 01                                                      Mar 01                           Apr 0

time                                                                                                                                                                                                                                                                             time

(a) Hubei                                                                                                                            (b) Italy

Figure 4: Covid 19 deaths

x$HubeI$weekday =<strong> format</strong>(x$HubeI$tIme, ‘<sup>°</sup>ha’) x$Italy$weekday =<strong> format</strong>(x$Italy$tIme, ‘<sup>°</sup>ha’) x$Italy$tImelnt =<strong> as</strong><strong>.numeric</strong>(x$Italy$tIme) x$HubeI$tImelnt =<strong> as</strong><strong>.numeric</strong>(x$HubeI$tIme) x$Italy$tImelId = x$Italy$tImelnt

x$HubeI$tImelId = x$HubeI$tIme

gamltaly = gamm4::<strong>gamm4</strong>(deaths – weekday +<strong> s</strong>(tImelnt, k=40), random = -(1itImeIId),

data=x$Italy, famIly=<strong>poisson</strong>(lInk=’log’))

gamHubeI = gamm4::<strong>gamm4</strong>(deaths – weekday +<strong> s</strong>(tImelnt, k=100), random = -(1itImeIId),

data=x$HubeI, famIly=<strong>poisson</strong>(lInk=’log’))

lme4::<strong>VarCorr</strong>(gamltaly$mer)

<table>

 <tbody>

  <tr>

   <td width="31">##</td>

   <td width="60">Groups</td>

   <td width="83">Name</td>

   <td width="441">Std.Dev.</td>

  </tr>

  <tr>

   <td width="31">##</td>

   <td width="60">tImelId</td>

   <td width="83">(Intercept)</td>

   <td width="441">0.10172</td>

  </tr>

  <tr>

   <td width="31">##</td>

   <td width="60">Xr</td>

   <td width="83">s(tImelnt)</td>

   <td width="441">1.51127</td>

  </tr>

 </tbody>

</table>




lme4::<strong>VarCorr</strong>(gamHubei$mer)

<table>

 <tbody>

  <tr>

   <td width="37">##</td>

   <td width="60">Groups</td>

   <td width="83">Name</td>

   <td colspan="6" width="541">Std.Dev.</td>

  </tr>

  <tr>

   <td width="37">##</td>

   <td width="60">timelid</td>

   <td width="83">(Intercept)</td>

   <td colspan="6" width="541">0.41303</td>

  </tr>

  <tr>

   <td width="37">##</td>

   <td width="60">Xr</td>

   <td width="83">s(timelnt)</td>

   <td colspan="6" width="541">3.44953</td>

  </tr>

  <tr>

   <td colspan="6" width="381">knitr::<strong>kable</strong>(<strong>cbind</strong>(<strong>summary</strong>(gamltaly$mer)$coef[,1:2],</td>

   <td colspan="2" width="332"><strong>summary</strong>(gamHubei$mer)$coef[,1:2]), digits=3)</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236"> </td>

   <td width="69">Estimate</td>

   <td width="76">Std. Error</td>

   <td width="68">Estimate</td>

   <td width="263">Std. Error</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">X(Intercept)</td>

   <td width="69">1.000</td>

   <td width="76">0.422</td>

   <td width="68">-1.493</td>

   <td width="263">1.148</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdayMon</td>

   <td width="69">0.464</td>

   <td width="76">0.123</td>

   <td width="68">-0.137</td>

   <td width="263">0.216</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdaySat</td>

   <td width="69">0.105</td>

   <td width="76">0.109</td>

   <td width="68">-0.069</td>

   <td width="263">0.212</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdaySun</td>

   <td width="69">-0.171</td>

   <td width="76">0.121</td>

   <td width="68">0.002</td>

   <td width="263">0.212</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdayThu</td>

   <td width="69">0.210</td>

   <td width="76">0.117</td>

   <td width="68">-0.470</td>

   <td width="263">0.221</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdayTue</td>

   <td width="69">0.081</td>

   <td width="76">0.141</td>

   <td width="68">-0.458</td>

   <td width="263">0.223</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">XweekdayWed</td>

   <td width="69">0.216</td>

   <td width="76">0.117</td>

   <td width="68">-0.038</td>

   <td width="263">0.214</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td colspan="4" width="236">Xs(timelnt)Fx1</td>

   <td width="69">4.426</td>

   <td width="76">1.661</td>

   <td width="68">4.760</td>

   <td width="263">3.949</td>

   <td width="7"> </td>

  </tr>

  <tr>

   <td width="29"></td>

   <td width="60"></td>

   <td width="83"></td>

   <td width="57"></td>

   <td width="69"></td>

   <td width="76"></td>

   <td width="68"></td>

   <td width="259"></td>

   <td width="7"></td>

  </tr>

 </tbody>

</table>




toPredict =<strong> data.frame</strong>(time =<strong> seq</strong>(<strong>as.Date</strong>(‘2020/1/1’),<strong> as.Date</strong>(‘2020/4/10′), by=’1 day’)) toPredict$timelnt =<strong> as</strong><strong>.numeric</strong>(toPredict$time)

toPredict$weekday = ‘Fri’

Stime =<strong> pretty</strong>(toPredict$time)

<strong>matplot</strong>(toPredict$time,

<strong>exp</strong>(<strong>do</strong><strong>.call</strong>(cbind, mgcv::<strong>predict</strong><strong>.gam</strong>(gamltaly$gam, toPredict, se.fit=TRUE)) % *% Pmisc::<strong>ciMat</strong>()),

col=’black’, lty=<strong>c</strong>(1,2,2), type=’l’, xaxt=’n’, xlab=”, ylab=’count’, ylim =<strong> c</strong>(0.5, 5000), xlim =<strong> as.Date</strong>(<strong>c</strong>(‘2020/2/20’, ‘2020/4/5’)))

<strong>axis</strong>(1,<strong> as.numeric</strong>(Stime),<strong> format</strong>(Stime, ‘%d %b’))

<strong>points</strong>(x$Italy[,<strong>c</strong>(‘time’,’deaths’)], col=’red’)

<strong>matplot</strong>(toPredict$time,

<strong>exp</strong>(<strong>do</strong><strong>.call</strong>(cbind, mgcv::<strong>predict</strong><strong>.gam</strong>(gamltaly$gam, toPredict, se.fit=TRUE)) % *% Pmisc::<strong>ciMat</strong>()),

col=’black’, lty=<strong>c</strong>(1,2,2), type= ‘l’, xaxt=’n’, xlab=”, ylab=’count’, ylim =<strong> c</strong>(0.5, 5000), xlim =<strong> as.Date</strong>(<strong>c</strong>(‘2020/2/20’, ‘2020/4/5′)), log=’y’)

<strong>axis</strong>(1,<strong> as.numeric</strong>(Stime),<strong> format</strong>(Stime, ‘%d %b’))

<strong>points</strong>(x$Italy[,<strong>c</strong>(‘time’,’deaths’)], col=’red’)

<strong>matplot</strong>(toPredict$time,

<strong>exp</strong>(<strong>do.call</strong>(cbind, mgcv::<strong>predict.gam</strong>(gamHubei$gam, toPredict, se.fit=TRUE)) % *% Pmisc::<strong>ciMat</strong>()), col=’black’, lty=<strong>c</strong>(1,2,2), type=’l’, xaxt=’n’, xlab=”, ylab=’count’,

xlim =<strong> as.Date</strong>(<strong>c</strong>(‘2020/1/20’, ‘2020/4/5’)))

<strong>axis</strong>(1,<strong> as.numeric</strong>(Stime),<strong> format</strong>(Stime, ‘%d %b’))

<strong>points</strong>(x$Hubei[,<strong>c</strong>(‘time’,’deaths’)], col=’red’)

<strong>matplot</strong>(toPredict$time,

<strong>exp</strong>(<strong>do.call</strong>(cbind, mgcv::<strong>predict.gam</strong>(gamHubei$gam, toPredict, se.fit=TRUE)) % *% Pmisc::<strong>ciMat</strong>()), col=’black’, lty=<strong>c</strong>(1,2,2), type=’l’, xaxt=’n’, xlab=”, ylab=’count’,

xlim =<strong> as.Date</strong>(<strong>c</strong>(‘2020/1/20’, ‘2020/4/5′)), log=’y’, ylim =<strong>c</strong>(0.5, 200))

<strong>axis</strong>(1,<strong> as.numeric</strong>(Stime),<strong> format</strong>(Stime, ‘%d %b’)) <strong>points</strong>(x$Hubei[,<strong>c</strong>(‘time’,’deaths’)], col=’red’)

<ol>

 <li>Write a down the statistical model corresponding to the gamm4 calls above, explaining in words what all of the variables are.</li>

 <li>Write a paragraph describing, in non-technical terms, what information the data analysis presented here is providing. Write text suitable for a short ‘Research News’ article in a University of Toronto news publication, assuming the audience knows some basic statistics but not much about non-parametric</li>

</ol>




modelling.

<ol start="3">

 <li>Explain, for each of the tests below, whether the test is a valid LR test and give reasons for your decision.</li>

</ol>

Hubei2 = gamm4::<strong>gamm4</strong>(deaths – 1 +<strong> s</strong>(timelnt, k=100), random = -(1Itimelid), data=x$Hubei, family=<strong>poisson</strong>(link=’log’), REML=FALSE)

Hubei3 = mgcv::<strong>gam</strong>(deaths – weekday +<strong> s</strong>(timelnt, k=100),

data=x$Hubei, family=<strong>poisson</strong>(link=’log’), method=’ML’)

Hubei4 = lme4::<strong>glmer</strong>(deaths – weekday + timelnt + (1Itimelid),

data=x$Hubei, family=<strong>poisson</strong>(link=’log’))

lmtest::<strong>lrtest</strong>(Hubei2$mer, gamHubei$mer)

## Likelihood ratio test

##

## Model 1:<sub> y</sub> – X – 1 + (1 I Xr) + (1 I timelid) ## Model 2:<sub> y</sub> – X – 1 + (1 I Xr) + (1 I timelid) ## #Df LogLik Df Chisq Pr(&gt;Chisq)

## 1                 4 -294.10

## 2 10 -289.03 6 10.136          0.1191

nadiv::<strong>LRTest</strong>(<strong>logLik</strong>(Hubei2$mer),<strong> logLik</strong>(gamHubei$mer), boundaryCorrect=TRUE)

## $lambda

## ‘log Lik.’ -10.13547 (df=4)

##

## $Pval

## ‘log Lik.’ 0.5 (df=4)

##

## $corrected.Pval

## [1] TRUE

lmtest::<strong>lrtest</strong>(Hubei3, gamHubei$mer)

## Warning in modelUpdate(objects[[i – 1]], objects[[i]]): original model was of ## class “gam”, updated model is of class “glmerMod”

## Likelihood ratio test

##

## Model 1: deaths – weekday + s(timelnt, k = 100)

## Model 2:<sub> y</sub> – X – 1 + (1 I Xr) + (1 I timelid)

##                     #Df LogLik              Df Chisq Pr(&gt;Chisq)

## 1 24.099 -300.45

## 2 10.000 -289.03 -14.099 22.84         0.06293 .

## —

## Signif. codes:     0 ‘ ***’ 0.001 ‘ **’ 0.01 ‘ *’ 0.05 ‘.’ 0.1 ‘ ‘ 1

nadiv::<strong>LRTest</strong>(<strong>logLik</strong>(Hubei3),<strong> logLik</strong>(gamHubei$mer), boundaryCorrect=TRUE)

## $lambda

## ‘log Lik.’ -22.8398 (df=24.09876)

##

## $Pval

## ‘log Lik.’ 0.5 (df=24.09876)

##

## $corrected.Pval

## [1] TRUE

lmtest::<strong>lrtest</strong>(Hubei4, gamHubei$mer)




## Likelihood ratio test

##

## Model 1: deaths – weekday + timelnt + (1 I timelid)

## Model 2:<sub> y</sub> – X – 1 + (1 I Xr) + (1 I timelid)

## #Df LogLik Df Chisq Pr(&gt;Chisq)

## 1          9 -376.02

## 2 10 -289.03 1 173.98 &lt; 2.2e-16 ***

## —

## Signif. codes:     0 ‘ ***’ 0.001 ‘ **’ 0.01 ‘ *’ 0.05 ‘.’ 0.1 ‘ ‘ 1

nadiv::<strong>LRTest</strong>(<strong>logLik</strong>(Hubei4),<strong> logLik</strong>(gamHubei$mer), boundaryCorrect=TRUE)

## $lambda

## ‘log Lik.’ -173.984 (df=9)

##

## $Pval

## ‘log Lik.’ 0.5 (df=9)

##

## $corrected.Pval

## [1] TRUE

lmtest::<strong>lrtest</strong>(Hubei2$mer, Hubei3)

## Warning in modelUpdate(objects[[i – 1]], objects[[i]]): original model was of ## class “glmerMod”, updated model is of class “gam”

## Likelihood ratio test

##

## Model 1:<sub> y</sub> – X – 1 + (1 I Xr) + (1 I timelid) ## Model 2: deaths – weekday + s(timelnt, k = 100) ## #Df LogLik Df Chisq Pr(&gt;Chisq)

## 1 4.000 -294.10

## 2 24.099 -300.45 20.099 12.704         0.8897

nadiv::<strong>LRTest</strong>(<strong>logLik</strong>(Hubei2$mer),<strong> logLik</strong>(Hubei3), boundaryCorrect=TRUE)

## $lambda

## ‘log Lik.’ 12.70433 (df=4)

##

## $Pval

## ‘log Lik.’ 0.0001824052 (df=4)

##

## $corrected.Pval

## [1] TRUE

<strong>References</strong>

Retnakaran, Ravi and Chang Ye (2020). “Outcome of the 2016 United States presidential election and the subsequent sex ratio at birth in Canada: an ecological study”. In:<em> BMJ Open</em> 10.2. DOT: 10.1136/bmjopen­2019-031208.




<table>

 <tbody>

  <tr>

   <td width="609">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

01 Mar                                            01 Apr                                                                                                       01 Mar                                          01 Apr

(a) Italy<strong>                                                                                                                                        </strong>(b) Italy




<table>

 <tbody>

  <tr>

   <td width="609">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="53">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●●<strong>●●</strong>●●<strong>●</strong></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="17">

    <table width="100%">

     <tbody>

      <tr>

       <td>●<strong>●</strong></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="21">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="18">

    <table width="100%">

     <tbody>

      <tr>

       <td><strong>●●</strong></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="17">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="20">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="15">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="20">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="12">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="24">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="18">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="12">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="24">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="39">

    <table width="100%">

     <tbody>

      <tr>

       <td>●● <strong>●</strong>●●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="15">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="15">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="18">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="20">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="52">

    <table width="100%">

     <tbody>

      <tr>

       <td>●● ●●●●<strong>●●</strong>●●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="15">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="20">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="18">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="17">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="21">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="24">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="14">

    <table width="100%">

     <tbody>

      <tr>

       <td>●●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<table>

 <tbody>

  <tr>

   <td width="11">

    <table width="100%">

     <tbody>

      <tr>

       <td>●</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




01 Feb                  01 Mar                     01 Apr                                                                               01 Feb                  01 Mar                   01 Apr

(c) Hubei                                                                                                                                   (d) Hubei

Figure 5: Predicted cases