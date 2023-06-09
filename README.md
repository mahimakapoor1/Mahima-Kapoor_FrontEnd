# Mahima-Kapoor_FrontEnd
Q1 Explain what the simple List component does?

The List component is a simple React component that displays a list of items and highlights the selected item. It is made up of two functional components, WrappedSingleListItem and WrappedListComponent, which receive props such as onClickHandler, text, and isSelected to render each list item.

Q2 What problems / warnings are there with code?

•	setSelectedIndex not defined proplerly.

      const [setSelectedIndex, selectedIndex] = useState();
      
=>

    const [selectedIndex, setSelectedIndex] = useState(null);
    
•	shapeOf and array are not  function.

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

            WrappedListComponent.defaultProps = {
               items: null,
            };
=>

    
             WrappedListComponent.defaultProps = {
            items: [{ text: "Item 1" },
            { text: "Item 2" },
            { text: "Item 3" },
            { text: "Item 4" }],
            };

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

             <li
                  style={{ backgroundColor: isSelected ? 'green' : 'red'}}
                  onClick={onClickHandler(index)}
                  >
  
=>

      const handleClick = () => {
            onClickHandler(index);
       };  

    return (
      <li
        style={{ backgroundColor: isSelected ? 'green' : 'red'}}
        onClick={handleClick}
      >

Q3 Modified code:

          import React, { useState, useEffect, memo, useCallback } from 'react';
      import PropTypes from 'prop-types';

             // Single List Item
            
            const WrappedSingleListItem = ({
            index,
            isSelected,
            onClickHandler,
            text,
            }) => {
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
      };

      WrappedSingleListItem.propTypes = {
      index: PropTypes.number,
      isSelected: PropTypes.bool,
      onClickHandler: PropTypes.func.isRequired,
      text: PropTypes.string.isRequired,
      };

      const SingleListItem = memo(WrappedSingleListItem);

      // List Component
        
      const WrappedListComponent = ({
      items
      }) => {
      const [selectedIndex, setSelectedIndex] = useState(null);

    useEffect(() => {
      setSelectedIndex(null);
    }, [items]);

    //  useCallback is a hook provided by React that returns a memoized version of a function,
    //  which can be used to optimize the performance of child components. 
    const handleClick = useCallback(
     index => {
        setSelectedIndex(index);
      },
      [setSelectedIndex]
    );

    return (
      <ul style={{ textAlign: 'left' }}>
        {items.map((item, index) => (
          <SingleListItem
            key={index}
            onClickHandler={() => handleClick(index)}
            text={item.text}
            index={index}
            isSelected={index===selectedIndex}
          />
        ))}
      </ul>
    )
      };

       WrappedListComponent.propTypes = {
            items: PropTypes.arrayOf(PropTypes.shape({
            text: PropTypes.string.isRequired,
            })),
            };

       
            WrappedListComponent.defaultProps = {
            items: [{ text: "Item 1" },
            { text: "Item 2" },
            { text: "Item 3" },
            { text: "Item 4" }],
            };

      const List = memo(WrappedListComponent);

      export default List;
