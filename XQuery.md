# XQuery Notes

## List files in a collection modified after a certain date

    let $fullCollection := xmldb:xcollection("/db/datasets/myapp/data")
    let $filteredCollection := xmldb:find-last-modfiied-since($fullCollection, xs:dateTime("2021-11-23T21:13:00"))
    return $filteredCollection

## List filename and specific element from files in a collection

    let $fullCollection := xmldb:xcollection("/db/datasets/myapp/data")
    return
        <table>
        {
            for $item in $fullCollection
                return
                    <tr>
                    {
                        concat("<td>", util:document-name($item), "</td><td>", data($item/starship/captain), "</td>")
                    }
                    </tr>
        }
        </table>
        
