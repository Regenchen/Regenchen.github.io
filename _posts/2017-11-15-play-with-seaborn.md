---
layout: post
title: Play with Seaborn
tags: Programming
---

[Seaborn](http://seaborn.pydata.org/index.html) is a Python visualization library based on matplotlib. It provides with themes, color palettes, and high-level built-in plot patterns to prettify your statistical graphics. But it's not omnipotent as well.

<!-- more -->

Following are the best practices(that I could figure out) with seaborn.

## Simply import seaborn

It's really important to remember that seaborn is based on and provides interface for matplotlib. Once seaborn is imported, the default aesthetic setup for matplotlib plots will be overridden by seaborn.

So the very first way to use seaborn is to __simply import it__.

Using only matplotlib:

{% highlight python %}
import matplotlib.pyplot as plt
plt.scatter('Attack', 'Defense', data=pokemon)
{% endhighlight %}

![import1](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-11-15-import-1.png){: .center-image}

Then we import seaborn and try again:

{% highlight python %}
import matplotlib.pyplot as plt
import seaborn as sns
plt.scatter('Attack', 'Defense', data=pokemon)
{% endhighlight %}

![import1](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-11-15-import-2.png){: .center-image}


It looks better without any sophisticated configurations.

## hue and color palettes

`hue` is a powerful parameter in seaborn. It simplifies the classification and coloration (if you want to) steps that you need to manually set up in matplotlib.

Using only matplotlib:

{% highlight python %}
# create color mapping
color = ['navy', 'limegreen', 'red']
mapping = dict(zip(pokemon.Stage.unique().tolist(), color))

# define apply function
def color_mapping(row):
    return mapping[row['Stage']]

# apply
pokemon['color'] = pokemon.apply(color_mapping, axis=1)

# plot
plt.scatter('Attack', 'Defense', data=pokemon, c='color')
{% endhighlight %}

Now with seaborn:

{% highlight python %}
sns.lmplot('Attack', 'Defense', data=pokemon, fit_reg=False, hue='Stage')
{% endhighlight %}

![hue](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-11-15-hue.png){: .center-image}

Wait, wait, wait. How did you configure colors in seaborn? Magic?

Nah, I haven't done yet. With `sns.set_palette()` you can set the default color palette for all plots. There are some built-in palettes available and this one is my favorite.

{% highlight python %}
sns.set_palette('colorblind')
sns.palplot(sns.color_palette())
{% endhighlight %}
![palette](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-11-15-palette.png){: .center-image}

## Customizing with matplotlib

Seaborn are equipped with some high-level complex plot functions such as `lmplot`, `jointplot`, `violinplot` and `swarmplot`. When using them, it's easier to configure axis, ticks, lims, titles and labels with our *old-skool* matplotlib commands.

{% highlight python %}
sns.lmplot('Attack', 'Defense', data=pokemon, fit_reg=False, scatter_kws={"s": 80}, size=8, hue='Stage')
sns.set(font_scale=1.5)
sns.plt.xlim(0, 150)
sns.plt.ylim(0, 200)
sns.plt.xticks(fontsize=14)
sns.plt.yticks(fontsize=14)
sns.plt.xlabel('Attack', fontsize=14)
sns.plt.ylabel('Defense', fontsize=14)
sns.plt.subplots_adjust(top=0.94)
sns.plt.suptitle('Attack versus defense', fontsize=16)
{% endhighlight %}

![config](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-11-15-config.png){: .center-image}

## Flaws

Unfortunately, we can't configure marker size with `matplotlib` anymore. In `seaborn` we can only pass float value, But in matplotlib we can pass a list and define marker size according to each data point instead.

So again, seaborn is good, but is good under specific circumstances. Before you switch to seaborn, think twice.

