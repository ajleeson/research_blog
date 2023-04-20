## More on "w" in Double-Periodic, Flat-Shelf Grid

## Running the Model As-Is

The model run is 15 minutes long. Info is written to history files every timestep (6 seconds).
Note that this model was run with 1x1 tiling via romsM. I also ran the model with romsS, and obtained the same results.

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/233496159-cb6b3aa0-ffd5-49b9-b148-cea0187cda4d.mp4" controls="controls" style="max-width: 550px;"></video><br>Fig. 1 Surface velocity and SSH in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/233496156-a8c8a476-5245-42dd-9a8c-c3af8293e348.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 2 Section salinity in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/233496162-40637371-bd4c-41a0-bebd-e89cb816503c.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 3 Section u in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/233496163-dd4ed18c-ef63-4112-bd74-58812fa38324.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 4 Section v in test case as-is.</p><br>

<p style="text-align:center;"><video src="https://user-images.githubusercontent.com/15829099/233496165-305bc8b1-bb34-4c91-b2f6-108415d78a8b.mp4" controls="controls" style="max-width: 800px;"></video><br>Fig. 5 Section w in test case as-is.</p><br>

---
## Key Observations

- w values are different between my run and John Wilkin's run
- w reaches a constant, steady value of -6.1e-4 m/s at the surface in my runs
  - This value is consistent with LwSrc discharge rate, except the sign is flipped:
  - $$\frac{10\  \mathrm{m}^3 \mathrm{s}^{-1}}{\text{Grid Area}} = \frac{10\  \mathrm{m}^3 \mathrm{s}^{-1}}{\big( 5000\  \mathrm{m} / 39\  \mathrm{gridcells}\big)^2} = 6.1\times10^{-4}\  \mathrm{m}\ \mathrm{s}^{-1}$$