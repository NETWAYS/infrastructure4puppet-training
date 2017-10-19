# Training

This training is designed as a three days hands-on training introducing Puppet. To continue
with programming with puppet take a look at the Fundamentals for Puppet and Puppet Advanced training. 

In the training you will get advanced knowlegde to integrate Puppet in your infrastructure.

Targeted audience are experienced Linux administrators in need of a configuration management
solution for their systems.

In addition to the sources you can find the rendered material on 
[netways.github.io](https://netways.github.io/NETWAYS/infrastructure4puppet-training)

* [Presentation](https://netways.github.io/infrastructure4puppet-training)
* [Handouts](https://github.com/NETWAYS/infrastructure4puppet-training/releases/download/v1.0/infrastructure4puppet-training-handouts.pdf)
* [Exercises](https://github.com/NETWAYS/infrastructure4puppet-training/releases/download/v1.0/infrastructure4puppet-training-exercises.pdf)
* [Solutions](https://github.com/NETWAYS/infrastructure4puppet-training/releases/download/v1.0/infrastructure4puppet-training-solutions.pdf)

## Provide your training

To run the presentation you will need [showoff 0.9.11.1](https://rubygems.org/gems/showoff/versions/0.9.11.1).
After installing it simply run `showoff serve` to get presenter mode with additional notes
and display window to present to your students.

For creating the rendered documents on your own run `showoff static print` (handouts),
`showoff static supplemental exercises` (exercises) or `showoff static supplemental solutions`
(solutions) followed by 
`wkhtmltopdf -s A5 --print-media-type --footer-left [page] --footer-right 'Infrastructure for Puppet Training' static/index.html handouts.pdf`

For some notes on setting up the training enviroment have a look at 'Setup.md'.

# Contribution

Patches to fix mistakes or add optional content are always appreciated. If you want to see
changes on the default content of the training we are open for suggestions but keep in mind
that the training is intended for a three day hands-on training.

The rendered content will be updated at least if we do a newer version of the material which
will also be tagged on git.

Material is licensed under [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-nc-sa/4.0/).
