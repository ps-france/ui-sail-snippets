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
a!localVariables(
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



### Card Navigation
#### Description
- Made with Card Layout
- Only available from 19.3


#### Preview
![alt text][cardNavigationPreview]

#### Code
```
a!localVariables(
  /** UI Parameters **/
  local!iconColor: "#EA7773",
  local!iconSize: "MEDIUM_PLUS",
  local!titleColor: "#EA7773",
  local!descriptionColor: "#333945",
  local!descriptionColorSelected: "#fff",
  local!itemBgColor: "#EAF0F1",
  local!itemBgColorSelected: "#616C6F",
  local!itemPerRow: 4,
  /* *****  */
  /** Content Parameters **/
  local!refDataTableNames: {
    {
      icon: "calendar",
      title: "Appian Events",
      description: "Event's organized by Appian for Customers"
    },
    {
      icon: "users",
      title: "Corporate User Repository",
      description: "Internal Employees Repository"
    },
    {
      icon: "gift",
      title: "Features",
      description: "Appian product features developed by Engineering"
    },
    {
      icon: "truck",
      title: "Appian Software Releases",
      description: "Appian's Software Releases"
    },
    {
      icon: "handshake-o",
      title: "Partners Organizations",
      description: "Appian's active partners"
    },
    {
      icon: "bullseye",
      title: "Engineering Annual goals",
      description: null
    }
  },
  /* *****  */
  /** Internal Parameters (DO NOT TOUCH) **/
  local!totalItems: count(
    local!refDataTableNames
  ),
  local!rows: quotient(
    local!totalItems,
    local!itemPerRow
  ),
  local!remainder: mod(
    local!totalItems,
    local!itemPerRow
  ),
  local!totalRows: if(
    local!remainder = 0,
    local!rows,
    local!rows + 1
  ),
  /* *****  */
  /** Dynamic Parameters **/
  local!selectedIndex: 1,
  /* *****  */
  {
    a!columnsLayout(
      columns: {
        a!columnLayout(
          contents: {
            a!forEach(
              items: repeat(
                local!totalRows,
                null
              ),
              expression: with(
                local!colIndex: fv!index,
                a!columnsLayout(
                  columns: {
                    a!forEach(
                      items: repeat(
                        local!itemPerRow,
                        null
                      ),
                      expression: a!localVariables(
                        local!itemIndex: (
                          local!colIndex - 1
                        ) * local!itemPerRow + fv!index,
                        local!item: index(
                          local!refDataTableNames,
                          local!itemIndex,
                          null
                        ),
                        a!columnLayout(
                          contents: {
                            a!cardLayout(
                              showBorder: false,
                              showWhen: not(
                                isnull(
                                  local!item.title
                                )
                              ),
                              contents: {
                                a!richTextDisplayField(
                                  labelPosition: "COLLAPSED",
                                  value: {
                                    a!richTextIcon(
                                      icon: local!item.icon,
                                      size: local!iconSize,
                                      color: local!iconColor
                                    ),
                                    char(
                                      10
                                    ),
                                    a!richTextItem(
                                      text: local!item.title,
                                      color: local!titleColor,
                                      style: "STRONG"
                                    ),
                                    char(
                                      10
                                    ),
                                    a!richTextItem(
                                      text: local!item.description,
                                      color: if(
                                        tointeger(
                                          local!selectedIndex
                                        ) = tointeger(
                                          local!itemIndex
                                        ),
                                        local!descriptionColorSelected,
                                        local!descriptionColor
                                      ),
                                      size: "SMALL",
                                      style: "EMPHASIS"
                                    )
                                  },
                                  align: "CENTER"
                                )
                              },
                              link: a!submitLink(
                                value: local!itemIndex,
                                saveInto: local!selectedIndex
                              ),
                              style: if(
                                tointeger(
                                  local!selectedIndex
                                ) = tointeger(
                                  local!itemIndex
                                ),
                                local!itemBgColorSelected,
                                local!itemBgColor
                              ),
                              height: "SHORT"
                            )
                          }
                        )
                      )
                    )
                  }
                )
              )
            )
          }
        )
      }
    )
  }
)
```





[sideItemMenuPreview]: https://raw.githubusercontent.com/ps-france/ui-sail-snippets/master/screenshots/side_item_menu.PNG "Side Item Menu"
[cardNavigationPreview]: https://github.com/ps-france/ui-sail-snippets/raw/master/screenshots/card_navigation.PNG "Card Navigation"