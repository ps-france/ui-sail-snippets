# Appian PS France | UI SAIL Snippets
This repository contains Appian Sail UI Code Snippets made by Appian PS France Team


### Side Item Menu
#### Description
- Made with Card Layout
- Only available from 19.3


#### Preview
![alt text][sideItemMenuPreview]

#### Code
```
load(
  /** UI Parameters **/
  local!menuTitleBgColor: "#EA7773",
  local!menuTitleColor: "#FFFFFF",
  local!menuItemBgColor: "#EAF0F1",
  local!menuItemBgColorSelected: "#DAE0E2",
  local!menuItemLabelColor: "#000",
  /* *****  */
  /** Contents Parameters **/
  local!title: "Menu",
  local!menuItem: {
    {
      icon: "home",
      label: "Accueil"
    },
    {
      icon: "line-chart",
      label: "Statistique"
    },
    {
      icon: "comments",
      label: "Commentaires"
    }
  },
  /* *****  */
  local!selectedMenuItem: 1,
  {
    a!columnsLayout(
      columns: {
        a!columnLayout(
          width: "NARROW",
          contents: {
            a!cardLayout(
              contents: {
                a!richTextDisplayField(
                  align: "LEFT",
                  labelPosition: "COLLAPSED",
                  value: a!richTextItem(
                    style: "STRONG",
                    color: local!menuTitleColor,
                    text: local!title
                  )
                )
              },
              showBorder: false,
              style: local!menuTitleBgColor
            ),
            a!forEach(
              items: local!menuItem,
              expression: a!cardLayout(
                contents: {
                  a!sideBySideLayout(
                    items: {
                      a!sideBySideItem(
                        width: "MINIMIZE",
                        item: a!richTextDisplayField(
                          labelPosition: "COLLAPSED",
                          value: a!richTextIcon(
                            icon: fv!item.icon
                          )
                        )
                      ),
                      a!sideBySideItem(
                        item: a!richTextDisplayField(
                          align: "LEFT",
                          labelPosition: "COLLAPSED",
                          value: a!richTextItem(
                            text: fv!item.label,
                            style: "STRONG",
                            color: local!menuItemLabelColor
                          )
                        )
                      )
                    }
                  )
                },
                style: if(
                  tointeger(
                    local!selectedMenuItem
                  ) = fv!index,
                  local!menuItemBgColorSelected,
                  local!menuItemBgColor
                ),
                showBorder: false,
                link: a!dynamicLink(
                  value: fv!index,
                  saveInto: local!selectedMenuItem,
                  showWhen: tointeger(
                    local!selectedMenuItem
                  ) <> fv!index
                )
              )
            )
          }
        ),
        a!columnLayout()
      }
    )
  }
)
```









[sideItemMenuPreview]: https://raw.githubusercontent.com/ps-france/ui-sail-snippets/master/screenshots/side_item_menu.PNG "Side Item Menu"
