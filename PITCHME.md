---

Fusion Patterns

---

Neos CMS - Monthly regulars' table in Zurich

---

```
prototype(Neos.NeosIo:Quote) < prototype(TYPO3.Neos:Content) {

	authorLinkLabel = ${String.pregMatch(this.authorLink, '/([a-z]+\.[a-z]+)/i')[0]}
	authorLinkLabel.@process.1 = ${String.indexOf(this.authorLink, 'https') >= 0 ? value + ' secure :)' : value + ' insecure :('}

	quotesQuery = ${q(site).find('[instanceof Neos.NeosIo:Quote][authorLink][authorLink!=""]')}

	imTheNewest = ${this.quotesQuery.sort('_creationDateTime', 'DESC').get(0).identifier == node.identifier}

	authorLinks = ${this.quotesQuery.get()}
	@context.authorLinks = ${this.authorLinks}

	authorLinksRendered = TYPO3.TypoScript:Collection {
		collection = ${authorLinks}
		itemName = 'itemNode'
#		itemRenderer = ${'<li>' + itemNode.properties.authorLink + '</li>'}
		itemRenderer = Neos.NeosIo:AuthorLinkItem
		@process.wrap = ${'<ul>' + value + '</ul>'}
	}

}

prototype(Neos.NeosIo:AuthorLinkItem) < prototype(TYPO3.TypoScript:Tag) {
	tagName = 'li'
	content = ${itemNode.properties.authorLink}
	content.@process.wrap = ${'<strong>' + value + '</strong>'}
	@if.notMine = ${itemNode.identifier != node.identifier}
}
```
