# Mahima-Kapoor_FrontEnd
Q1 Explain what the simple List component does?

The List component is a simple React component that displays a list of items and highlights the selected item. It is made up of two functional components, WrappedSingleListItem and WrappedListComponent, which receive props such as onClickHandler, text, and isSelected to render each list item. 

Q2 What problems / warnings are there with code?

•	setSelectedIndex is not defined properly.

	const [setSelectedIndex, selectedIndex] = useState();

=>

	const [selectedIndex, setSelectedIndex] = useState(null);

•	shapeOf is not a function and array is not a function.

	WrappedListComponent.propTypes = {
 	 items: PropTypes.array(PropTypes.shapeOf({
    	  text: PropTypes.string.isRequired,
  	})),
	};
=>

	WrappedListComponent.propTypes = {
         items: PropTypes.arrayOf(PropTypes.shape({
          text: PropTypes.string.isRequired,
        })),
        };

•	Cannot read properties of null (reading 'map') at WrappedListComponent.

	{items.map((item, index) => (		
=>

	{items && items.map((item, index) => (

WARNINGS:
•	Each child in a list should have a unique "key" prop.

 	<SingleListItem
	onClickHandler={() => handleClick(index)}
	text={item.text}
	index={index}
	isSelected={selectedIndex}
	/>
	
=>

	<SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={index===selectedIndex}
        />
•	Cannot update a component (`WrappedListComponent`) while rendering a different component (`WrappedSingleListItem`).

	return (
    	 <li
      	style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      	onClick={onClickHandler(index)}
    	>
      	{text}
    	</li>
  	);
	
=>

	const handleClick = () => {
    	onClickHandler(index);
  	};

  	return (
    	<li
      	style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      	onClick={handleClick}
    	>
      	{text}
    	</li>
  	);

Q3 Please fix, optimize, and/or modify the component as much as you think is necessary.
	
	
