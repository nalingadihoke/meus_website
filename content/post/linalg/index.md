---
title: Pythonic applications of Linear Algebra
summary: Face recognition using Principal Component Analysis
tags: [ML]
categories: [PCA, linalg, ML, numpy]
date: "2020-11-30"
share: false

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: 
  focal_point: Smart

links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
#slides: example
---
As the title suggests, this project saw me extend some of my linear algebra knowledge with inspiration from a [course](https://faculty.math.illinois.edu/~phierony/math415-2020.html) I took in 2020. Here, simple face recognition is demonstrated, a timeseries is analyzed and other interesting applications are discussed.

Principal Component Analysis ([PCA](https://en.wikipedia.org/wiki/Principal_component_analysis)) is a way of capturing most of the variance in the data (in an orthogonal basis). It reduces dimensions and transforms the data linearly into new properties that are not correlated. Singular Value Decomposition ([SVD](https://jonathan-hui.medium.com/machine-learning-singular-value-decomposition-svd-principal-component-analysis-pca-1d45e885e491)) will be utilized to diagonalize the matrices from which the basis vectors will be truncated to give us our principal components.

#  Equations

Assuming a $m \\times n$ matrix $X$, the principal components are defined as the eigenvectors of the dataset’s covariance matrix. Assuming $\\hat{X}$ is the dataset centered at the origin, it can be said that $\\hat{X}^{T}\\hat{X}$ is proportional to the covariance matrix which means finding the eigenvectors for $\\hat{X}^{T}\\hat{X}$ is enough. These are represented by the columns of V in the reduced SVD of $\\hat{X}$, 

$$\hat{X} = U\Sigma V^{T}$$


# <a name="face"></a>Face Recognition

For this analysis I used, AT&T Laboratories Cambridge's “[Database of Faces](https://web.archive.org/web/20180802044943/http:/www.cl.cam.ac.uk/research/dtg/attarchive/facedatabase.html)”, a set of gray scale face images (like in the image above) normalized to the same resolution. Each image is a flattened row of a larger ‘faces’ matrix. First, to zero center the faces matrix, an average face is computed and subtracted from each row of the matrix. The ‘average’ face is shown below.

{{< figure src="combined_face.png" title="" lightbox="true" >}}

Using `SVD`, we can calculate the eigenbasis of the desired covariance matrix. 

```sh
U, S, Vt = la.svd(faces_zero_centered, full_matrices=False)
V = Vt.T
```

As a side note, the principal components of any one of the images in the training set can be captured by converting them into the eigenface basis using the V vector found above. 

{{< figure src="not_trained.png" title="" lightbox="true" >}}

Now, with an unknown face (left) that the model is not trained on, first we will subtract the average face from it. The resulting image (right) will then be converted to the eigenface basis by a simple [linear transformation](https://textbooks.math.gatech.edu/ila/linear-transformations.html#:~:text=A%20linear%20transformation%20is%20a,n%20and%20all%20scalars%20c%20.). 
```sh
unknown_zero_centered = face_unknown - face_avg
unknown_basis = unknown_zero_centered @ V
faces_basis = faces_zero_centered @ V
```

To match an existing image, the `“closest”` face in the face basis will be selected on the basis of least distance between the vectors.

```sh
n = 0
differences= la.norm(faces_basis - unknown_basis, axis=1)
n = np.argmin(differences)
plt.imshow(faces[n].reshape(face_shape), cmap="gray")
```

{{< figure src="prediction.png" title="prediction" lightbox="true" >}}

The above demonstration was a simple implementation of PCA on a data set of images where its assumed each face fills out a similar area in the image. Check out this [link](https://pythonmachinelearning.pro/face-recognition-with-eigenfaces/) for a more complex implementation involving a trained neural net and scikit-learn


# Short Example of Timeseries

{{< figure src="time_combined.png" title="" lightbox="true" >}}

`PCA` can be used to split up various timeseries too. In the image below, the temperature data for six US cities is plotted (top left). Next, The average is subtracted to zero center the data like in the steps above (top right). Finally, the data is broken into its top two PCs (bottom). 

```sh
# zero center the data
temp_avg = np.mean(temperature,axis=0)
temp_zero_center = temperature - temp_avg

# SVD breakdown
U,S, Vt = la.svd(temp_noavg)
V = Vt.T

# plotting the first two eigenvectors
plt.figure(figsize=(20,10))
lines = plt.plot((V[:,:2] ), '-', )
plt.legend(iter(lines), map(lambda x: f"PC {x}", range(1,6)))
```

Since average temperature dips in the winter and peaks in the summer, the first component represents climates that remains relatively static year-round.

Further Reading:

1.	Markov Matrics was one of the coolest take aways in linalg. [This](https://towardsdatascience.com/brief-introduction-to-markov-chains-2c8cab9c98ab) article breaks down the concept and its applications in data science.
2.	Briefly mentioned in the above article, Google [PageRank](https://en.wikipedia.org/wiki/PageRank) utilizes a special kind of square matrix called the [Google Matrix](https://en.wikipedia.org/wiki/Google_matrix)
3.	Sandeep Khurana explains Linear Regression quite eloquently [here]((https://towardsdatascience.com/linear-regression-with-example-8daf6205bd49)).
