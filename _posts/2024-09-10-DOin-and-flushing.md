## DOin and Flushing Time

This week I tried to wade through the science soup of my results. As a reminder, I have **two-layer budget results in 21 inlets for the year 2017** (in addition to the 5 inlets from 2014).

I have made a lot of time series, a lot of bar charts, and a lot of scatter plots. For brevity and focus, I am only showing the figures that help illustrate what I think is going on in these inlets. Of course, there is more to uncover, and I am happy to share any other figures as needed.

More details below.

---
## Interface depths

A quick aside on interface depths: for the most part, I used an interface which was 1/3 of the depth. For shallower inlets (i.e. 12 m deep), the interface is at 1/2 the depth, and for the super shallow inlets (i.e. 5 m deep), the interface depth is 1 m from the bottom.

There is probably a better way to divide the layers, but for now this is the method I have gone ahead with.

---
## Deep layer DO vs. Bottom sigma-layer DO

We are interested in the lowest DO concentrations in Puget Sound. From my prior work, we found that the lowest DO concentrations occur in the bottom sigma layer. However, the two-layer budget framework tells us about the DO transport in and out of a "deep layer," rather than the bottom sigma layer. 

As a quick check, I verified that there is a strong relationship between the deep layer DO and the bottom sigma-layer DO. Figure 1 shows a scatter plot of the monthly mean bottom sigma-layer DO vs. the monthly mean deep layer DO in all 21 inlets for the year 2017. As it turns out, there is a strong linear correlation between the bottom sigma-layer DO and the deep layer DO, suggesting that it is appropriate to study the deep layer DO in our analysis. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/5a9bb873-7179-4916-a9b3-5c526f6a6e51" width="350"/><br>Fig 1. 2017 monthly mean bottom sigma-layer DO vs. monthly mean deep layer DO in all 21 inlets across Puget Sound. Inlets are colored by the basin to which they belong.</p><br>

---
## Important scatter plots

Results from last week suggested that the inflowing exchange flow DO concentration (hereafter called TEF DOin) plays an important role in establishing the DO concentration within the deep layer of the inlet. To confirm this result more rigorously, I made *a lot* of scatter plots. My goal was to identify whether any other variables had a stronger relationship to deep layer DO than TEF DOin. 

The variables I considered include:
- Deep layer DO
- TEF DOin
- Tflush [flushing time = (inlet volume) / (Qin from TEF)]
- Recirculation transport (sum of TEF exchange and vertical fluxes)
- DO consumption due to biological processes
- Mean inlet depth
- Inlet volume
- Aspect ratio; L/W (after approximating the inlet as a rectangle)

Rather than showing the results from all of these scatter plots, I will instead share just the most important ones. 

The strongest relationship I identified was between the mean monthly deep layer DO to the mean monthly TEF DOin (Fig 2).

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/449c0b7a-3537-4f2a-8ba5-d4e5f5b76f36" width="350"/><br>Fig 2. 2017 monthly mean deep layer DO vs. monthly mean TEF DOin for all 21 inlets across Puget Sound. Inlets are colored by the basin to which they belong.</p><br>

Though TEF DOin has a strong correlation with deep layer DO, there remains some scatter in this figure. 

Figure 3 shows this exact same scatter plot, but now the dots are colored by inlet flushing time. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/53f3eff0-a9ef-4b27-9fd4-a3f863db5fa9" width="400"/><br>Fig 3. 2017 monthly mean deep layer DO vs. monthly mean TEF DOin, colored by inlet flushing time, for all 21 inlets across Puget Sound. Inlets are colored by the basin to which they belong.</p><br>

Inlets with very short residence times are more likely to have mean deep layer DO that matches the TEF DOin concentrations, because what comes in gets renewed quickly. Inlets with longer residence times tend to have lower DO than the TEF DOin, presumably because there was time for the oxygen in the inflow to be consumed before being replenished.

Note that from my scatter plot analysis (not shown), I found that monthly mean Tflush had the next highest correlation to monthly mean deep layer DO, second to TEF DOin. (r = 0.66).

These results suggest that a combination of TEF DOin and Tflush establish the mean deep layer DO concentrations within an inlet. As a next step, I created a multiple linear regression (MLR) model to predict deep layer DO concentrations from TEF DOin and Tflush. However, it is apparently important for the predictor variables in the MLR model to have little correlation. Thus, I checked the correlation between TEF DOin and Tflush (Fig 4.)

Figure 4 shows that there is a significant correlation between TEF DOin and Tflush, but this correlation is modest (R2 = 0.15). Thus, I continued with the MLR analysis. 

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/c8aafc05-bf55-4ca0-9a4e-7ec3d09472db" width="400"/><br>Fig 4. 2017 monthly mean deep layer DO vs. monthly mean TEF DOin, colored by inlet flushing time, for all 21 inlets across Puget Sound. Inlets are colored by the basin to which they belong.</p><br>

---
## Multiple linear regression (MLR) model results

I made many decisions for the MLR model. They are summarized below:

I removed Hammersley Inlet from the analysis, because it had high DO concentrations (mean Aug/Sep deep layer DO of > 9 mg/L). Not only did Hammersley Inlet appear like a high-DO outlier amongst the inlets, but from prior model-data comparison I found that LiveOcean tends to overpredict DO in super shallow inlets like Hammersley. Since we are ultimately interested in low DO, I opted to remove Hammersley from the analysis.

I fit two different MLR models to the data. When I used a single MLR model, I found that there tended to be larger error in the deepest inlets. Since then, I have fit a different MLR model to shallow inlets (shallower than 14.5 m) and deeper inlets (deeper than 14.5 m). As I also discovered, the deep layer DO in shallow inlets do not seem to have a strong dependence on Tflush (the MLR coefficient for Tflush was 0). Thus, I technically fit a simple linear regression between deep layer DO and TEF DOin for the shallow inlets.

The cutoff of 14.5 m roughly splits the inlets in half.

Shallower than 14.5 m:
- Similk Bay
- Oak Harbor
- Hammersley Inlet (omitted)
- Henderson Inlet
- Killsut Harbor
- Eld Inlet
- Budd Inlet
- Totten Inlet
- Sinclair Inlet
- Quartermaster Harbor
- Dyes Inlet

Deeper than 14.5 m:
- Crescent Bay
- Penn Cove
- Case Inlet
- Lynch Cove
- Carr Inlet
- Holmes Harbor
- Port Susan
- Elliot Bay
- Commencement Bay
- Dabob Bay

I fit the MLR models to 2017 monthly mean values of deep layer DO, TEF DOin, and Tflush. Then, I used the MLR models to predict daily values of deep layer DO. The resulting time series are shown in Figure 5. The colored lines are the raw LiveOcean output, and the thin black line are the MLR model prediction.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/a4ff6104-1060-4d67-b70a-164a62105ae1" width="900"/><br>Fig 5. Time series of daily mean deep layer DO. The colored lines come from raw LiveOcean output. The thin black lines are the MLR model predicted DO concentrations.</p><br>

The residuals for these time series are shown in Figure 6.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/547d9240-4404-4d63-853d-04b5c01b1e59" width="900"/><br>Fig 6. Time series of residuals (actual-predicted) for MLR model results shown in Figure 5.</p><br>

As a final performance test, I plotted the MLR model predicted mean Aug/Sep deep layer DO vs. the LiveOcean mean Aug/Sep deep layer DO. The intention of this figure was to assess how well the MLR model predicts hypoxia or high oxygen in all 20 inlets during the summer months. The results are shown in Figure 7.

<p style="text-align:center;"><img src="https://github.com/user-attachments/assets/a1daf525-9204-4e67-8fe2-994d3ac0018f" width="450"/><br>Fig 7. MLR model vs. raw LiveOcean 2017 mean Aug/Sep deep layer DO.</p><br>

---
## Discussion

Based on these results, I have found that TEF DOin and Tflush play a dominant role in establishing the deep layer DO concentrations in Puget Sound terminal inlets.

However, these two varaiables alone are not the entire story. There is more going on.

Although the MLR model does a decent job with long-term averages (Fig 7.), it has high frequency variability for daily predictions (see Fig. 5 and Fig 6.). This may be because I fit the MLR model to monthly averaged data, but could also indicate that I am missing some sort of damping process. For example, daily variability in Qin will result in daily variabiliy in Tflush, but flushing time is itself a duration, and the amount of time that water spends in the inlet is unlikey to immediately respond to high daily variability in Qin.

The MLR model also seems to struggle more with deep inlets such as Dabob Bay, Carr Inlet, and Lynch Cove, to name a few. There must be other processes in these inlets that I am not capturing.

Finally, the elephant in the room is Holmes Harbor (the outlier green dot in Figure 7). Even after taking an Aug/Sep average of DO concentrations, the MLR model truly fails to represent DO in Holmes Harbor. 

Though I did not find a strong correlation between bio consumption rate and deep layer DO, I wonder whether there is a seasonal cycle in the strength of DO consumption in the inlets. Perhaps Tflush should have a stronger negative coefficient during the growing season, and a near-zero coeffecient during the winter. 

Another big question: what sets the DO concentration of the inflowing exchange flow??

I would love to hear your thoughts as well.


