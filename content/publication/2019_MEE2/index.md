+++
title = "Incorporating fine‐scale environmental heterogeneity into broad‐extent models"

date = "2019-04-08"

authors = ["Laura J. Graham", "Rebecca Spake", "Simon Gillings", "Kevin Watts", "Felix Eigenbrod"]

publication_types = ["2"]

featured = true

publication = "Methods in Ecology and Evolution"

abstract = """
1. A key aim of ecology is to understand the drivers of ecological patterns, so that we can accurately predict the effects of global environmental change. However, in many cases, predictors are measured at a finer resolution than the ecological response. We therefore require data aggregation methods that avoid loss of information on fine‐grain heterogeneity.

2. We present a data aggregation method that, unlike current approaches, reduces the loss of information on fine‐grain spatial structure in environmental heterogeneity for use with coarse‐grain ecological datasets. Our method contains three steps: (a) define analysis scales (predictor grain, response grain, scale‐of‐effect); (b) use a moving window to calculate a measure of variability in environment (predictor grain) at the process‐relevant scale (scale‐of‐effect); and (c) aggregate the moving window calculations to the coarsest resolution (response grain). We show the theoretical basis for our method using simulated landscapes and the practical utility with a case study. Our method is available as the grainchanger r package.

3. The simulations show that information about spatial structure is captured that would have been lost using a direct aggregation approach, and that our method is particularly useful in landscapes with spatial autocorrelation in the environmental predictor variable (e.g. fragmented landscapes) and when the scale‐of‐effect is small relative to the response grain. We use our data aggregation method to find the appropriate scale‐of‐effect of land cover diversity on Eurasian jay Garrulus glandarius abundance in the UK. We then model the interactive effect of land cover heterogeneity and temperature on G. glandarius abundance. Our method enables us quantify this interaction despite the different scales at which these factors influence G. glandarius abundance.

4. Our data aggregation method allows us to integrate variables that act at varying scales into one model with limited loss of information, which has wide applicability for spatial analyses beyond the specific ecological context considered here. Key ecological applications include being able to estimate the interactive effect of drivers that vary at different scales (such as climate and land cover), and to systematically examine the scale dependence of the effects of environmental heterogeneity in combination with the effects of climate change on biodiversity."""

summary = "How can we aggregate data avoiding loss of information on fine‐grain heterogeneity?"

doi = "https://doi.org/10.1111/2041-210X.13177"

image_preview = ""
selected = false
projects = ["scalefores"]
tags = ["scale", "data-aggregation", "grainchanger"]
url_pdf = "https://besjournals.onlinelibrary.wiley.com/doi/epdf/10.1111/2041-210X.13177"
url_preprint = ""
url_code = "https://doi.org/10.5281/zenodo.2588304"
url_dataset = "https://doi.org/10.5281/zenodo.2588304"
url_project = ""
url_slides = ""
url_video = ""
url_poster = ""
url_source = ""
math = true
highlight = true
[header]
image = ""
caption = ""
+++
